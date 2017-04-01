# イメージファイルをCentOS7に設定
FROM centos:7.3.1611

# 作成者記述
MAINTAINER tsukasa <t14159265358979323846@gmail.com>

# 実行コマンド設定
RUN rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
RUN yum -y update nginx-release-centos
RUN yum -y --enablerepo=nginx install nginx
RUN mv /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.bk
RUN yum -y install epel-release
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
RUN yum install -y --enablerepo=remi,remi-php70 php php-devel php-mbstring php-pdo php-gd php-mysql php-mcrypt
RUN yum install -y php-fpm --enablerepo=epel --enablerepo=remi --enablerepo=remi-php70
RUN mv /etc/php-fpm.d/www.conf /etc/php-fpm.d/www.conf.bk
RUN mv /etc/php.ini /etc/php.ini.bk
RUN systemctl enable php-fpm
RUN systemctl enable nginx

# 設定ファイル上書き
ADD default.conf /etc/nginx/conf.d/
ADD www.conf /etc/php-fpm.d/www.conf
Add php.ini /etc/php.ini

# 起動時実行コマンド
CMD ["/sbin/init"]
