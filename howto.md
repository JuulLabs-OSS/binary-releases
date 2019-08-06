### HOW TO: CREATE THE NEWT@1.7 FORMULA

# Two repos:
1. ~/repos/binary-releases (https://github.com/JuulLabs-OSS/binary-releases)
2. ~/repos/homebrew-mynewt (https://github.com/JuulLabs-OSS/homebrew-mynewt)

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

# Create the bottle.
```
brew uninstall --force mynewt-newt
brew install --build-bottle mynewt-newt
brew bottle mynewt-newt
```

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

# Push binaries.
```
mkdir -p ~/repos/binary-releases/mynewt-newt-tools_1.7.0
cp mynewt-newt--1.7.0.high_sierra.bottle.tar.gz ~/repos/binary-releases/mynewt-newt-tools_1.7.0/
```
Create a PR from ~/repos/binary-releases/mynewt-newt-tools_1.7.0.

# Push formulae.
```
cp /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt.rb ~/repos/homebrew-mynewt/
cp /usr/local/Homebrew/Library/Taps/juullabs-oss/homebrew-mynewt/mynewt-newt@1.7.rb ~/repos/homebrew-mynewt/
```
Create a PR from ~/repos/homebrew-mynewt.
