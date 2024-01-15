FROM mcr.microsoft.com/devcontainers/base:bullseye

RUN apt update && apt install git make nano

# install nodenv in /opt/nodenv/bin
RUN git clone https://github.com/nodenv/nodenv.git /opt/nodenv \
 && cd /opt/nodenv \
 && ./src/configure \
 && make -C src \
 && mkdir -p /opt/nodenv/plugins \
 && git clone https://github.com/nodenv/node-build.git /opt/nodenv/plugins/node-build

# Install pnpm
RUN curl -fsSL https://get.pnpm.io/install.sh | PNPM_HOME=/opt/pnpm/bin bash -

# add nodenv init code to profile.d
COPY <<-EOT /etc/profile.d/50-nodenv.sh
export PATH="/opt/nodenv/bin:/opt/pnpm/bin:$PATH"

eval "$(nodenv init - bash)"

if [ ! -e "$(nodenv root)/plugins" ]; then
    mkdir -p "$(nodenv root)"
    ln -s /opt/nodenv/plugins "$(nodenv root)/plugins"
fi
EOT

# add nice-to-have to profile.d
COPY <<-EOT /etc/profile.d/50-nice-to-have.sh
alias pn=pnpm
alias rm="rm -i"
alias mv="mv -i"
alias cp="cp -i"

git config --global alias.co checkout
git config --global alias.b branch
git config --global alias.s status
git config --global alias.c commit
git config --global alias.cp cherry-pick
EOT


