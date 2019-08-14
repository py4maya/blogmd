---
title: rpm 包 编译和生成
date: 2019-08-14 10:29:28
tags: [rpm,linux]
categories: 工具
---

## 生成rpmbuild 基础目录

```shell
rpmbuild # 默认生成在/root/rpmbuild 目录
```

## 一个简单的spec 文件，用来编译rpm

```shell

Name:           niffler
Version:        0.2.6
Release:        1%{?dist}
Summary:        niffler
  
Group:          Development/Tools  
License:        GPL  
Source0:        %{name}-%{version}.tar.gz  
  
BuildRequires:    libpcap-devel
Requires:         libpcap-devel
  
%description  
  
%prep  
%setup -q  
  
%build  

%install  
mkdir -p ${RPM_BUILD_ROOT}/bin
mkdir -p ${RPM_BUILD_ROOT}/etc/
mkdir -p ${RPM_BUILD_ROOT}/etc/init.d/
install -m 755 niffler ${RPM_BUILD_ROOT}/bin/niffler
./niffler echo service > ${RPM_BUILD_ROOT}/etc/init.d/niffler

%post
if [ -f /etc/niffler.json ] 
then
    cp /etc/niffler.json /etc/niffler_old.json
fi
niffler echo config > /etc/niffler.json
chmod 755 ${RPM_BUILD_ROOT}/etc/init.d/niffler
chkconfig --add niffler
killall niffler
service niffler start

%clean
rm -rf $RPM_BUILD_ROOT

%files
/bin/niffler
/etc/init.d/niffler

```

## 编译生成rpm

```shell
 version=$(cat niffler.spec  | grep Version | awk '{print $2}')
 echo "build rpm package version $version"
 killall niffler
 rm -rf build/*
 rpmbuild_dir=/root/rpmbuild
 source_dir=`pwd`
 
 go build -o build/niffler -ldflags "-s -w"
 mkdir -p build
 rpmdev-setuptree
 mkdir -p ${rpmbuild_dir}/SOURCES/niffler-${version}
 cp build/niffler ${rpmbuild_dir}/SOURCES/niffler-${version}
 cd ${rpmbuild_dir}/SOURCES/
 tar czvf niffler-${version}.tar.gz niffler-${version}
 cd ${source_dir}
 cp niffler.spec ${rpmbuild_dir}/SPECS/niffler.spec
 rpmbuild -bb ${rpmbuild_dir}/SPECS/niffler.spec
 cp ${rpmbuild_dir}/RPMS/x86_64/*.rpm build/
 rm -rf ${rpmbuild_dir}

 go clean 
 rm -rf build/niffler
```

## 发布rpm,nginx 配置文件

```shell

    location /secrepo {
        autoindex on;  
        root /data0/www/secrepo/public;
    }

```

## 生成repo 相关文件

```shell
 createrepo /data0/www/secrepo/public/secrepo/
```
