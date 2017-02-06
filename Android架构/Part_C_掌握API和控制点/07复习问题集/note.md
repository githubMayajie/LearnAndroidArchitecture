# 1区别核心服务和APP服务
Android 里有2种服务
1. 应用层的应用服务，统称位SDK-based Service  
2. 系统层的核心服务，统称为Core Service  

## App服务
APP服务是开机完成后，用户加载并开启某APP时，才会启动该APP里的服务，它常会定义成为Service的一个子类别  
