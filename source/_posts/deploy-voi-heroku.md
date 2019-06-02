---
title: Deploy sử dụng Heroku
date: 2019-06-01 15:21:19
tags: lalala
---

Heroku cung cấp dịch vụ máy chủ đám mây giúp dễ dàng trong việc deploy ứng dụng. Điều tuyệt vời ở Heroku là trang này bạn có thể sử dụng dịch vụ hoàn toàn miễn phí với các ứng dụng web không yêu cầu phải có tốc độ truy cập cao hay dung lượng lớn.

Trong bài học này chúng ta sẽ
  + Tạo một dự án trên heroku
  + Cấu hình git server repository cho phía local
  + Deploy ứng dụng

### Tạo dự án trên heroku

Để tạo dự án trên Heroku, bạn cần tạo một tài khoản (hoàn toàn miễn phí) sau đó kích hoạt và đăng nhập vào địa chỉ https://dashboard.heroku.com/. Sau khi đăng nhập vào địa chỉ này, bạn sẽ thấy có một icon hình dấu cộng dùng để tạo dự án. Click vào icon này và chọn Create new app:

![alt text](https://i.imgur.com/OiXd6go.png "heroku deploy")

Trong mục App name chọn tên cho ứng dụng nếu bạn để trống Heroku sẽ chọn ngẫu nhiên một tên cho ứng dụng. Tiếp theo có 2 lựa chọn để chọn vùng cho server Heroku sẽ host ứng dụng là Mỹ và Châu Âu. Với các ứng dụng ở Việt Nam thì bạn nên chọn Europe để nâng cao tốc độ truy cập. Sau đó, bấm Create app.
![alt text](https://i.imgur.com/S1Up7Xu.png "Create app heroku")


### Tạo Git Repository Trên Máy Local

Bây giờ trên máy tính local, chúng ta cần khởi tạo Git local repository (hay local repo). Local repo giống như một cái kho, nơi chứa source code mà bạn sẽ dùng để phát triển dụng. Trên terminal di chuyển tới thư mục ứng dụng ở máy tính bạn và chạy câu lệnh sau:

``` bash
  $ git init
```

Câu lệnh này dùng để khởi tạo một dự án Git ở trên máy tính local (hay còn gọi là local repo). Tiếp theo chạy câu lệnh sau để xem trạng thái (status) của các file trên local repo này:

``` bash
  $ git status
```

Bạn sẽ thấy kết quả như sau:
``` bash
  # On branch master
  #
  # Initial commit
  #
  # Untracked files:
  #   (use "git add <file>..." to include in what will be committed)
  #
  #   node_modules/
  #   npm-debug.log
  #   package.json
  nothing added to commit but untracked files present (use "git add" to track)
```

Ở đây, Git nói với bạn có 2 file và 1 thư mục chưa được theo dõi (tracked) và để thêm chúng vào bạn cần chạy câu lệnh git add.

Tuy nhiên, khi làm việc với ứng dụng Nodejs nói chung và Expressjs framework nói riêng, chúng ta không nên thêm file npm-debug.log và thư mục node_modules/ vào trong Git vì nội dung của 2 file này sẽ khác nhau phụ thuộc vào hệ điều hành mỗi lập trình viên sử dụng. Để Git không theo doi 2 file này, chúng ta tạo một file .gitignore với nội dung như sau:

```bash
/npm-debug.log
/node_modules/
```

Để thêm tất cả các file hiện có trong thư mục của ứng dụng. Bây giờ nếu bạn chạy câu lệnh git statusbạn sẽ chỉ thấy Git thông báo 1 file cần được thêm vào là package.json:

```bash
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   .gitignore
#   package.json
```

Tiếp theo chạy câu lệnh sau để thêm tất cả các file có trong thư mục dự án vào Git:

```bash
$ git add .
```

Chạy git status một lần nữa để kiểm tra trạng thái của các file. Lúc này bạn sẽ thấy Git đánh dấu các file vừa được thêm vào bằng màu khác với trước đó:

![alt text](https://i.imgur.com/GIdBjii.jpg "github")

Cuối cùng, chúng ta hãy tạo một commit đầu tiên sử dụng câu lệnh:

```bash
$ git commit -m 'First commit'
```

ở đây, option -m được sử dụng để tạo message cho commit. Message này thường là nội dung tóm tắt mô tả xem commit thay đổi những gì trong source code. Vì đây là commit đầu tiên nên tôi chỉ đơn giản để là First commit.

### Cấu Hình Git Repository

Khi bạn tạo một ứng dụng sử dụng giao diện web trên trang Heroku, lúc này Heroku sẽ tạo cho bạn sẽ có một Git server repository (repo server). Repo server là một kho chứa code nhưng nó khác với local ở chỗ mọi lập trình viên trong dự án đều có thể để có thể đấy (push) source code lên trên đó hoặc kéo (pull) code của người khác về để cập nhật những thay đổi tạo ra bởi người khác. Để tìm thấy địa chỉ đường dẫn cho repository trên server Heroku này, vào trong trang quản lý app (dùng giao diện web trên Heroku) bạn click vào tab Settings. Ở đây bạn sẽ tìm thấy địa chỉ URL này, của tôi như sau:

```bash
git@heroku.com:hoclaptrinh-expressjs-mongo.git
```

Tiếp theo, để cấu hình URL này là server repo cho local repo, chạy câu lệnh sau:

```bash
$ git remote add origin git@heroku.com:hoclaptrinh-expressjs-mongo.git
```

Để kiểm tra lại thông tin của repo server xem có chính xác hay chưa, chạy câu lệnh:

```bash
$ git remote -v
```
Ở đây option -v là viết tắt của từ verbose

### Deploy Ứng Dụng

Để deploy ứng dụng lên Heroku, bạn cần cài đặt Heroku Toolbelt. Bạn có thể xem hướng dẫn về cách cài đặt Heroku Toolbelt ở bài viết Cài đặt và sử dụng Heroku Toolbelt.

Sau khi cài đặt xong Heroku Toolbelt bạn cần thêm ssh key để có thể đấy source code lên server. Để thêm ssh key chạy câu lệnh sau:

```bash
$ heroku keys:add
```

Cuối cùng bạn có thể deploy source code lên Heroku bằng cách chạy câu lệnh:


```bash
$ git push -u origin master
```

Khi đấy souce code lần đầu tiên lên trên server bạn nên sử dụng option -uđể Git hiểu rằng repo server này sẽ theo dõi master branch của repo local. Trong các lần push source code tiếp theo bạn có thể làm đơn giản hơn bằng câu lệnh:

```bash
$ git push origin
```

Lúc này Git hiểu rằng bạn muốn đẩy source code trong master local branch lên origin remote brach mà không cần phải thêm master vào trong câu lệnh.
Sau khi kết thúc việc push source code, Heroku sẽ tự động deploy ứng dụng cho bạn.

<div style="text-align: right">**Nguồn: Code Hub**</div>
