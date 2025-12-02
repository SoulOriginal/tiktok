FROM ubuntu:22.04

ARG FTP_USER=ftpuser
ARG FTP_PASSWORD=changeme

RUN apt-get update && apt-get install -y \
    vsftpd \
    openssh-server \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -m -d /home/${FTP_USER} -s /bin/bash ${FTP_USER} && \
    echo "${FTP_USER}:${FTP_PASSWORD}" | chpasswd && \
    mkdir -p /home/${FTP_USER}/video /home/${FTP_USER}/images && \
    chown -R ${FTP_USER}:${FTP_USER} /home/${FTP_USER}

RUN mkdir /var/run/sshd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

RUN echo "listen=YES" > /etc/vsftpd.conf && \
    echo "listen_ipv6=NO" >> /etc/vsftpd.conf && \
    echo "anonymous_enable=NO" >> /etc/vsftpd.conf && \
    echo "local_enable=YES" >> /etc/vsftpd.conf && \
    echo "write_enable=YES" >> /etc/vsftpd.conf && \
    echo "local_umask=022" >> /etc/vsftpd.conf && \
    echo "dirmessage_enable=YES" >> /etc/vsftpd.conf && \
    echo "use_localtime=YES" >> /etc/vsftpd.conf && \
    echo "xferlog_enable=YES" >> /etc/vsftpd.conf && \
    echo "connect_from_port_20=YES" >> /etc/vsftpd.conf && \
    echo "chroot_local_user=YES" >> /etc/vsftpd.conf && \
    echo "allow_writeable_chroot=YES" >> /etc/vsftpd.conf && \
    echo "secure_chroot_dir=/var/run/vsftpd/empty" >> /etc/vsftpd.conf && \
    echo "pam_service_name=vsftpd" >> /etc/vsftpd.conf && \
    echo "pasv_enable=YES" >> /etc/vsftpd.conf && \
    echo "pasv_min_port=21100" >> /etc/vsftpd.conf && \
    echo "pasv_max_port=21110" >> /etc/vsftpd.conf && \
    echo "userlist_enable=YES" >> /etc/vsftpd.conf && \
    echo "userlist_file=/etc/vsftpd.userlist" >> /etc/vsftpd.conf && \
    echo "userlist_deny=NO" >> /etc/vsftpd.conf

RUN mkdir -p /var/run/vsftpd/empty

ARG FTP_USER
RUN echo "${FTP_USER}" > /etc/vsftpd.userlist

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

EXPOSE 21 22 21100-21110

CMD ["/entrypoint.sh"]
