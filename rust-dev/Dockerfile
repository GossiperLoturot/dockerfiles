FROM archlinux:base

# key & package update
RUN pacman -Sy --noconfirm archlinux-keyring && pacman -Su --noconfirm

# install common packages
RUN pacman -S --noconfirm git zsh fzf exa bat ripgrep fd neovim

# install dev packages
RUN pacman -S --noconfirm clang mold sccache rust rust-src cargo

# set rust cache & linker
ENV RUSTC_WRAPPER=sccache
ENV RUSTFLAGS="-C linker=clang -C link-arg=-fuse-ld=mold"

# personalize
RUN git clone https://github.com/GossiperLoturot/dotfiles ~/.dotfiles; \
    ~/.dotfiles/install.sh
RUN zsh ~/.zshrc
RUN nvim --headless -c qa; \
    nvim --headless -c "autocmd User PackerComplete qa" -c PackerSync; \
    nvim --headless -c "TSInstallSync rust" \
    -c "MasonInstall rust-analyzer" -c qa

WORKDIR /work
EXPOSE 3000

CMD ["/usr/sbin/zsh"]