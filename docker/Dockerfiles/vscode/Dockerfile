FROM ubuntu:jammy

EXPOSE 8080

USER root

RUN apt-get update && apt-get install -y \
    bat \
    btop \
    ca-certificates \
    curl \
    git \
    iputils-ping \
    nodejs \
    npm \
    ranger \
    sl \
    software-properties-common \
    speedtest-cli \
    tldr \
    tree \
    unzip \
    wget \
    zsh

RUN ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

# Add root to Group www-data
RUN usermod -aG www-data root

# Install ZSH
RUN chsh -s /bin/zsh && sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended \
    && git clone https://github.com/zsh-users/zsh-autosuggestions.git /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions \
    && git clone https://github.com/zsh-users/zsh-completions.git /root/.oh-my-zsh/custom/plugins/zsh-completions \
    && git clone https://github.com/denysdovhan/spaceship-prompt.git /root/.oh-my-zsh/custom/themes/spaceship-prompt \
    && ln -s /root/.oh-my-zsh/custom/themes/spaceship-prompt/spaceship.zsh-theme /root/.oh-my-zsh/custom/themes/spaceship.zsh-theme \
    && sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-completions)/' /root/.zshrc \
    && sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="spaceship"/' /root/.zshrc

# Install Neovim
RUN add-apt-repository ppa:neovim-ppa/unstable && apt-get update \
    && apt-get install -y neovim

# Install PHP 8.0
RUN apt-get update && \
    apt-get install -y ca-certificates && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update && \
    apt-get install -y php8.1 php8.1-cli php8.1-common php8.1-mbstring php8.1-mysql php8.1-xml php8.1-zip && \
    update-alternatives --set php /usr/bin/php8.1

# Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');" && \
    echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >> ~/.zshrc

# Code Server
RUN curl -fsSL https://code-server.dev/install.sh | sh

# Install Jetbrains Mono Font
RUN apt-get update && \
    apt-get install -y fonts-jetbrains-mono

# Install PHPStan
RUN composer global require phpstan/phpstan

# Install Cypress
RUN apt-get update && \
    apt-get install -y libgtk2.0-0 libgtk-3-0 libnotify-dev libgconf-2-4 \
    libgbm-dev libnss3 libxss1 libasound2 libxtst-dev xvfb && \
    npm install -g cypress

# Install PHPUnit
RUN composer global require phpunit/phpunit

# Install PHP CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"  
RUN /root/.config/composer/vendor/bin/phpcs --config-set installed_paths /root/.composer/vendor/phpcompatibility/php-compatibility
RUN /root/.config/composer/vendor/bin/phpcs --config-set default_standard PSR12

# PHPMD
RUN curl -fsSL https://github.com/phpmd/phpmd/releases/download/2.9.1/phpmd.phar --output /usr/local/bin/phpmd && \
    chmod +x /usr/local/bin/phpmd

# Install VSCode Extensions
RUN code-server --install-extension bmewburn.vscode-intelephense-client && \
    code-server --install-extension felixfbecker.php-debug && \
    code-server --install-extension devsense.composer-php-vscode && \
    code-server --install-extension recca0120.vscode-phpunit && \
    code-server --install-extension esbenp.prettier-vscode && \
    code-server --install-extension wongjn.php-sniffer && \
    code-server --install-extension ryanluker.vscode-coverage-gutters && \
    code-server --install-extension Shelex.vscode-cy-helper && \
    code-server --install-extension mblode.twig-language-2 && \
    code-server --install-extension johnbillion.vscode-wordpress-hooks && \
    code-server --install-extension Gruntfuggly.todo-tree && \
    code-server --install-extension EditorConfig.EditorConfig && \
    code-server --install-extension PKief.material-icon-theme && \
    code-server --install-extension enkia.tokyo-night

RUN echo "alias vi=nvim" >> /root/.zshrc && \
    echo "alias vim=nvim" >> /root/.zshrc && \
    echo "alias nah='git reset --hard && git clean -df'" >> /root/.zshrc && \
    echo "alias wip='git add . && git commit -m \"wip\"'" >> /root/.zshrc 

# Start Code-Server
CMD ["code-server", "--bind-addr", "0.0.0.0:8080", "--auth", "none", "--user-data-dir", "/root/.local/share/code-server", "/workspace"]
