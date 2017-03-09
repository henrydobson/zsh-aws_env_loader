# AWS Environment Loader for ZSH
## With Prezto

```bash
mkdir -p ~/.zfunc
git clone git@github.com:henrydobson/zsh-aws_env_loader.git ~/.zfunc/aws
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

## Configuration and usage

```bash
mkdir -p ~/.aws
touch ~/.aws/{config,credentials}

# ~/.aws/config
[profile example]
region = eu-west-1

# ~/.aws/credentials
[example]
aws_access_key_id = <redacted>
aws_secret_access_key = <redacted>

# Usage
aws_set_env # tab to complete
Loading profile: example - |DONE|
```
