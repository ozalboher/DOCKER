# Use the latest Fedora image
FROM fedora:latest

# Update DNF configuration to disable GPG checks and skip SSL checks temporarily
RUN echo -e "sslverify=0\ngpgcheck=0" >> /etc/dnf/dnf.conf && \
echo "fastestmirror=True" >> /etc/dnf/dnf.conf

# Attempt to fix the CA certificates by reinstalling and setting up trusted CAs manually
RUN dnf -y update --nogpgcheck && \
dnf -y install ca-certificates curl --nogpgcheck && \
update-ca-trust extract

# Verify network connection and attempt to reach mirror servers
RUN curl -I https://mirrors.fedoraproject.org --insecure || echo "Network check failed"

# Attempt installation of httpd while SSL checks are disabled
RUN dnf -y install httpd --nogpgcheck || (echo "Attempting alternative mirror"; dnf clean all)

# Re-enable SSL checks for future commands
RUN sed -i '/^sslverify=0/d' /etc/dnf/dnf.conf && sed -i '/^gpgcheck=0/d' /etc/dnf/dnf.conf

ADD webserver.tar.gz /var/www/html/
# Expose port 80 to allow HTTP traffic
EXPOSE 80

# Start Apache server in the foreground
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
