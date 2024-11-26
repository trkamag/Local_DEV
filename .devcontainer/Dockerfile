
FROM mcr.microsoft.com/devcontainers/universal:2-linux

WORKDIR /opt/app

COPY requirements.txt /opt/app

RUN apt-get update && \
    pip install --no-cache-dir --upgrade pip && \
    pip install -r requirements.txt

ARG USERNAME=oliana
ARG USER_UID=1001
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME 

#RUN chown -R oliana:oliana /workspaces/Local_DEV

# Download and install Terraform
# RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
#     https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
#     sudo tee /etc/apt/sources.list.d/hashicorp.list

# https://developer.hashicorp.com/terraform/install
# https://medium.com/@gyulshenabazz/mastering-terraform-with-visual-studio-code-dev-containers-5294e3f5bfaa
RUN wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    tee /etc/apt/sources.list.d/hashicorp.list

# Install Terraform
RUN apt update \
    &&  apt-get install terraform
# Add information to avoid failure of vscode docker extension
#RUN chmod 666 /var/run/docker.sock

# Install terraform autocomplete
RUN terraform -install-autocomplete

# Install AWSCLI2
RUN curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip \
    &&  unzip awscliv2.zip \
    &&  ./aws/install

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME

RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true

# Modify ZSH theme
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/' /home/$USERNAME/.zshrc

# Add alias to bashrc
RUN echo  '\n# Added by Thierry K. on 2024114\n' >> /home/$USERNAME/.zshrc

RUN echo  "alias ll='ls -alF'\nalias la='ls -A'\nalias l='ls -CF'\nalias tf='terraform'" >> /home/$USERNAME/.zshrc

CMD ["zsh"]
