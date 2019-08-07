##### HOW TO: CREATE THE NEWT@1.7 FORMULA

# Three repos:
1. ~/repos/binary-releases (https://github.com/JuulLabs-OSS/binary-releases)
2. ~/repos/homebrew-mynewt (https://github.com/JuulLabs-OSS/homebrew-mynewt)
3. ~/go/src/mynewt.apache.org/newt (https://github.com/apache/mynewt-newt)

### Create the binary bottle

# Create the newt binary
Note: We have to do this by hand rather than with `brew install --build-bottle`.  This is necessary for the resulting binary to contain the correct git hash in its `newt version` output.

```
cd ~/go/src/mynewt.apache.org/newt
git fetch origin --tags
git checkout mynewt_1_7_0_tag
./build.sh
sudo cp newt/newt /usr/local/Cellar/mynewt-newt
```

Make sure the resulting build is clean
```
/usr/local/Cellar/mynewt-newt/newt version
```
(ensure a git hash is displayed without the word "dirty").

# Create to working directory and cd to it
Any directory will do.  The remainder of these steps are run from this directory.
```
mkdir /tmp/newt17
cd /tmp/newt17
```

# Create the bottle.
brew bottle mynewt-newt

# Push binary.
I don't know why, but the bottle filename contains an extra dash.  Remove it with the `cp` command below.
```
mkdir -p ~/repos/binary-releases/mynewt-newt-tools_1.7.0
cp mynewt-newt--1.7.0.high_sierra.bottle.tar.gz ~/repos/binary-releases/mynewt-newt-tools_1.7.0/mynewt-newt-1.7.0.high_sierra.bottle.tar.gz
```
Create a PR from ~/repos/binary-releases/mynewt-newt-tools_1.7.0.

### Create the formulae

# Setup
```
wget https://github.com/apache/mynewt-newt/archive/mynewt_1_7_0_tag.tar.gz
shasum -a 256 mynewt_1_7_0_tag.tar.gz   # Remember the output of this command.
```

# Edit /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt.rb.
1. Replace 1.6 with 1.7
2. Replace 1_6 with 1_7
3. In the `bottle do` block, replace the sha256 with that in the output from the `brew bottle` command.
4. Replace the top level "sha256" field with the shasum output (previous shell command).

# Create version formula.
```
cp /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt@1.6.rb /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt@1.7.rb
```

# Edit /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt@1.7.rb.
1. Replace 1.6 with 1.7
2. Replace 1_6 with 1_7
3. In the `bottle do` block, replace the sha256 with that in the output from the `brew bottle` command.
4. Replace the top level "sha256" field with the shasum output.
5. Change MynewtNewtAT16 to MynewtNewtAT17.

# Push formulae.
```
cp /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt.rb ~/repos/homebrew-mynewt/
cp /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt@1.7.rb ~/repos/homebrew-mynewt/
```
Create a PR from ~/repos/homebrew-mynewt.
