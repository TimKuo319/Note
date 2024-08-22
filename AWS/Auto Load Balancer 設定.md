
1. 建立 `load balancer` 
	+ 設定 load balancer 要導向的 `target group`
	+ 設定要導向 `target group` 的 port
	+ 設定 load balancer 要監聽的 port
	+ 設定 load balancer 的 `security group`

2. 建立 `target group`
	+ 設定要 group 要進行 `health check` 的 `endpoint`
	+ 指定 `group` 內的目標 type
		+ ec2
		+ ...
		+ ...
	+ 