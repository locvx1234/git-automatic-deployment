# git-automatic-deployment

### Mô hình

 repo : /var/repo/site.git

 directory cần deploy: /var/www/domain.com

 
NOTE: Với Gitlab, repo được tạo ở `/var/opt/gitlab/git-data/repositories/$USER/repository_name`

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
	echo "Start deploy"
	git --work-tree=/var/www/domain.com --git-dir=/var/repo/site.git checkout -f
	echo "Finish deploy"
	
Xem thêm : Code push theo branch xác định : 
Cấp permission cho file 

	chmod +x post-receive

### Máy local

Tạo một folder git 

```sh
mkdir project && cd project
git init
```

Cấu hình remote tới repo trên server deploy `live`  

	$ git remote add live ssh://user@mydomain.com/var/repo/site.git
	echo "Hello " >> README.md
	$ git add *
	$ git commit -m "My project is ready"
	$ git push live master
	
Kết quả 

	$ git push live master
	Counting objects: 5, done.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (3/3), 301 bytes | 0 bytes/s, done.
	Total 3 (delta 0), reused 0 (delta 0)
	remote: Start deploy
	remote: Finish deploy
	To ssh://user@mydomain.com/var/repo/site.git
		4b052f6..6ad7de4  master -> master
		

### Tham khảo 

https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps

Xem thêm về Git Hooks : https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks




