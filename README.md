# AWS Environment Loader for ZSH
## With Prezto

```bash
mkdir -p ~/.zfunc
git clone ... ~/.zfunc/aws
cat << EOF >> ~/.zpreztorc
# Custom scripts 

autoload -U compinit && compinit

if [ -d $HOME/.zfunc ]; then
        fpath=($HOME/.zfunc "${fpath[@]}")
        if [ -d $HOME/.zfunc/aws ]; then
                fpath=($HOME/.zfunc/aws "${fpath[@]}")
                autoload aws_set_env aws_clear_env _profile_completion
                compdef _profile_completion aws_set_env
        fi
fi
EOF
```
