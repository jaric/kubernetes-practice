# Use the official image as a parent image
FROM ubuntu
 
# Update the system
RUN apt-get update && apt-get upgrade -y
 
# Install OpenSSH Server
RUN apt-get install -y openssh-server 

# Install required packages
RUN apt-get install -y bash net-tools vim less net-tools curl gpg kmod software-properties-common 
 
# Set up configuration for SSH
RUN mkdir /var/run/sshd
RUN echo 'root:pass' | chpasswd
RUN chsh -s /bin/bash root
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN service ssh restart
 
# SSH login fix. Otherwise, user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
 
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
 
# Expose the SSH port
EXPOSE 22

COPY src/init.sh /tmp/init.sh
RUN chmod u+x /tmp/init.sh
 
# Run SSH
CMD ["/usr/sbin/sshd", "-D"]

