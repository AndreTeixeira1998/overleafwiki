On Debian:

create unprivileged user:
```
		groupadd sharelatex
		useradd -g sharelatex -d /opt/sharelatex sharelatex	
```
	
start using that user
```
		screen -U -S sharelatex
		sudo -u sharelatex grunt run 
```
or to suppress logging:
```
                sudo -u sharelatex grunt run > /dev/null
```