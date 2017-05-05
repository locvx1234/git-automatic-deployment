# git-automatic-deployment

### Mô hình

 repo : /var/repo/site.git

 directory cần deploy: /var/www/domain.com

### Tạo một repo

```sh
cd /var
mkdir repo && cd repo
mkdir site.git && cd site.git
git init --bare
```

Tùy chọn `bare` nghĩa là folder không có các tập tin nguồn, chỉ để kiểm soát phiên bản

### Hooks 

Trong repo có một folder `hooks`, nó chứa các file mẫu cho phép bạn có thể tùy chỉnh các hành động.

Có 3 loại hooks :  `pre-receive`, `post-receive` và `update`.

`pre-receive` thực thi ngay khi server nhận được `push` , `update` tương tự nhưng thực thi một lần cho mỗi nhánh, còn `post-receive` thực thi khi `push` hoàn tất.

Chúng ta cần quan tâm đến `post-receive`

	cd hooks
	cat > post-receive
	
Thêm nội dung sau vào `post-receive`, ấn Ctrl+D để ghi nhận 

	#!/bin/sh
	git --work-tree=/var/www/domain.com --git-dir=/var/repo/site.git checkout -f

Cấp permission cho file 

	chmod +x post-receive

### Máy local

Tạo một folder git 

```sh
mkdir project && cd project
git init
```

Cấu hình remote tới repo trên server remote với nhánh `live` 

	git remote add live ssh://user@mydomain.com/var/repo/site.git
	echo "Hello " >> README.md
	git add *
	git commit -m "My project is ready"
	git push live master
	
	

	


