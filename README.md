# AWS Environment Loader for ZSH

## With Prezto

## Requirements

- zsh
- gnupg
- Yubikey (optional)

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

# ~/.aws/ecredentials
[example]
aws_access_key_id = <redacted>
aws_secret_access_key = <redacted>

# encrypt ~/.aws/ecredentials
gpg -ear youruid@gpgkey.com --output ~/.aws/ecredentials ~/.aws/credentials

# confirm readable
gpg -dq ~/.aws/ecredentials

# yes? remove plain text file
rm ~/.aws/credentials

# Usage
aws_set_env # tab to complete & enter gnupg pin
Loading profile: example - |DONE|
```

## Known issues

Tab completion.<br>
There's an issue with loading the completions for this loader. I've isolated it to the following conditional in the prezto completions module.

```bash
autoload -Uz compinit
_comp_files=(${ZDOTDIR:-$HOME}/.zcompdump(Nm-20))
if (( $#_comp_files )); then
  compinit -i -C
else
  compinit -i
fi
```

To fix this, comment out the conditional.

```bash
autoload -Uz compinit
_comp_files=(${ZDOTDIR:-$HOME}/.zcompdump(Nm-20))
# if (( $#_comp_files )); then
#   compinit -i -C
# else
#   compinit -i
# fi
```
