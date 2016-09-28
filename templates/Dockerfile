# https://hub.docker.com/_/centos/ systemd integration
FROM {{ base_image_name }}
MAINTAINER "{{ maintainer_name }}" <{{ maintainer_mail_address }}>
# これどういう意味？
ENV container docker
VOLUME [ "/sys/fs/cgroup" ]

RUN yum -y install passwd openssh openssh-server openssh-clients sudo rsync git vim
RUN echo 'root:{{ root_password }}' | chpasswd
# create user
RUN useradd {{ work_user_name}}
RUN echo '{{ work_user_name }}:{{ work_user_password}}' | chpasswd
# Set up SSH
# 鍵認証なしでもこれ必要なのか？
RUN mkdir -p /home/{{ work_user_name }}/.ssh; chown {{ work_user_name }} /home/{{ work_user_name }}/.ssh; chmod 700 /home/{{ work_user_name }}/.ssh && \
    echo "公開鍵" > /home/{{ work_user_name }}/.ssh/authorized_keys && \
    chown docker /home/{{ work_user_name }}/.ssh/authorized_keys && \
    chmod 600 /home/{{ work_user_name }}/.ssh/authorized_keys
# setup sudoers
RUN echo "{{ work_user_name }} ALL=(ALL) ALL" >> /etc/sudoers.d/{{ work_user_name }}
# Set up SSHD config
RUN sed -ri 's/#PermitRootLogin yes/PermitRootLogin yes/g' /etc/ssh/sshd_config && \
    sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config && \
    sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config && \
    # PAMを利用するときには下記をオフにしないとSSHログインできない
    # Centos7からPAMをオフにするのが推奨されてないので、下記設定は必須？
    sed -i -e 's/^\(session.*pam_loginuid.so\)/#\1/g' /etc/pam.d/sshd

# 起動時にポート指定すれば不要なようだ
#EXPOSE 22
#EXPOSE 80

CMD ["/usr/sbin/init"]
