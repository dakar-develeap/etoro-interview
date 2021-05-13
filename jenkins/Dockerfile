FROM jenkins/jenkins:lts

USER root

# Install tools
RUN \
    # Docker
    curl -sSL https://get.docker.com/ | sh && \
    usermod -aG docker jenkins && \
    # Azure cli
    curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash && \
    # Kubectl
    az aks install-cli && \
    # Helm
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 && \
    chmod 700 get_helm.sh && \
    ./get_helm.sh && rm get_helm.sh

USER jenkins
