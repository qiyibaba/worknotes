# 远程SSH一键登录脚本

```shell
#!/bin/sh

pwd=`dc -e 7803407977933779508P`
run=${1}
host=${2}

#main()
{
#check host
if [ "x${host}" == "x" ];then
 echo "ip can't be blank,please check!"
 exit 1
fi

echo ${host} | grep -E  "^([0-9]{1,3}.){3}[0-9]{1,3}$" >/dev/null
if [ $? -ne 0 ];then
 echo "$1 not a correct ip,please check!"
 exit 1
fi

ping -c 1 -t 3 ${host} >/dev/null
if [ $? -ne 0 ];then
 echo "ping $1 failed,please check net!"
 exit 1
fi

if [ "${run}" == "ssh" ];then
 sshpass -p $pwd ssh ningyue.lt@${host} -o "StrictHostKeyChecking no"
elif [ "${run}" == "scp" ];then
 if [ "x${3}" != "x" ];then
  if [ -f ${3} ] || [ -d ${3} ];then
   # make temp dir
   d="ny_`date +%Y%m%d%H%M%S`"
   sshpass -p $pwd ssh ningyue.lt@${host} "mkdir ~/${d}"
   
   #upload
   echo "upload file ${3} to dictionary ~/${d}"
   sshpass -p $pwd scp -r ${3} ningyue.lt@${host}:~/${d}
  else
   echo "upload file can't be null or ${3} is not exists,please check!"
   exit 1
  fi

 else
  echo "${3} is null!"
  exit 1
 fi
fi
}
```

