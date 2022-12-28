FROM archlinux:base

# key & package update
RUN pacman -Sy --noconfirm archlinux-keyring && pacman -Su --noconfirm

# install common packages
RUN pacman -S --noconfirm git zsh fzf exa bat ripgrep fd neovim

# install dev packages
RUN pacman -S --noconfirm gcc rustup

# install rust toolchain 1.42.0 with rust-src
RUN rustup toolchain install 1.42.0 -c rust-src

# create symbolic link for rust-analyzer
RUN ln -s ~/.rustup/toolchains/1.42.0-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src \
    ~/.rustup/toolchains/1.42.0-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library

# install cargo-compe binary
RUN curl -OL https://github.com/qryxip/cargo-compete/releases/download/v0.10.4/cargo-compete-v0.10.4-x86_64-unknown-linux-gnu.tar.gz; \
    tar -xzf cargo-compete-v0.10.4-x86_64-unknown-linux-gnu.tar.gz ./cargo-compete-v0.10.4-x86_64-unknown-linux-gnu/cargo-compete; \
    mkdir -p ~/.cargo/bin; \
    mv cargo-compete-v0.10.4-x86_64-unknown-linux-gnu/cargo-compete ~/.cargo/bin; \
    rm -r cargo-compete-v0.10.4-x86_64-unknown-linux-gnu.tar.gz cargo-compete-v0.10.4-x86_64-unknown-linux-gnu

# personalize
RUN git clone https://github.com/GossiperLoturot/dotfiles ~/.dotfiles; \
    ~/.dotfiles/install.sh
RUN zsh ~/.zshrc
RUN nvim --headless -c qa; \
    nvim --headless -c "autocmd User PackerComplete qa" -c PackerSync; \
    nvim --headless -c "TSInstallSync rust" -c "MasonInstall rust-analyzer" -c qa

WORKDIR /work
EXPOSE 3000

CMD ["/usr/sbin/zsh"]