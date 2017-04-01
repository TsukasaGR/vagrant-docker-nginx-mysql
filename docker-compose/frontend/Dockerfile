# イメージファイルをCentOS7に設定
FROM centos:7.3.1611

# 作成者記述
MAINTAINER tsukasa <t14159265358979323846@gmail.com>

# 実行コマンド設定
RUN rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
RUN yum -y update nginx-release-centos
RUN yum -y --enablerepo=nginx install nginx
RUN mv /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.bk
RUN systemctl enable nginx

ADD default.conf /etc/nginx/conf.d/

# 起動時実行コマンド
CMD ["/sbin/init"]

