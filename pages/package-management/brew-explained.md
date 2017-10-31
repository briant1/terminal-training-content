## Brew Explained

First a few terms:

Term        | Description           
------------- | ------------- 
Formula     | The package definition 
Keg      | The installation prefix of a Formula      
Cellar | All Kegs are installed here      
Tap | A Git repository of Formulae and/or commands      
Bottle | Pre-built Keg used instead of building from source      
[view more](https://docs.brew.sh/Formula-Cookbook.html#homebrew-terminology)

Homebrew installs packages into the /usr/local directory. Why use /usr/local ?
* It’s easier
* /usr/local/bin is already in your PATH.
* Apple has left this directory for us. Which means there is no /usr/local directory by default, so there is no need to worry about messing up existing tools.\[[1](https://docs.brew.sh/FAQ.html)\]

Brew is based off ruby and git. All the core files as well as the formulae (packages) are in the /usr/local directory. These formulae are Ruby files that describe the software they install and contain instructions to install and test it.

Calling brew update does a git pull and gives you a nice overview
![brew output](https://s3.us-east-2.amazonaws.com/terminal-training/public/terminal-training-content/brew-update.png)

Formulas are ruby classes which define how packages are installed. Homebrew installs to the Cellar and then symlinks some of the installation into /usr/local so that other programs can see what’s going on. 

Here's an example of wget's formula 'brew edit wget'
<pre>
class Wget < Formula
  desc "Internet file retriever"
  homepage "https://www.gnu.org/software/wget/"
  url "https://ftp.gnu.org/gnu/wget/wget-1.19.2.tar.gz"
  mirror "https://ftpmirror.gnu.org/wget/wget-1.19.2.tar.gz"
  sha256 "4f4a673b6d466efa50fbfba796bd84a46ae24e370fa562ede5b21ab53c11a920"
  bottle do
    sha256 "4f6896bbed75ea89f04d849357390b32e7462b86c8a3063dca85c9f5f7db1aaa" => :high_sierra
    sha256 "d8b3ae9836eed0145615ca95132e76784a93c0f4a1f43a5b4f4af49b712d3020" => :sierra
    sha256 "ab41d6f569535c4a19a64ea3f03f477818d66e56a86f8b9e1631311f5a4cca44" => :el_capitan
  end

  head do
    url "https://git.savannah.gnu.org/git/wget.git"

    depends_on "autoconf" => :build
    depends_on "automake" => :build
    depends_on "xz" => :build
    depends_on "gettext"
  end

  deprecated_option "enable-debug" => "with-debug"

  option "with-debug", "Build with debug support"

  depends_on "pkg-config" => :build
  depends_on "pod2man" => :build if MacOS.version <= :snow_leopard
  depends_on "openssl@1.1"
  depends_on "pcre" => :optional
  depends_on "libmetalink" => :optional
  depends_on "gpgme" => :optional

  def install
    args = %W[
      --prefix=#{prefix}
      --sysconfdir=#{etc}
      --with-ssl=openssl
      --with-libssl-prefix=#{Formula["openssl@1.1"].opt_prefix}
    ]

    args << "--disable-debug" if build.without? "debug"
    args << "--disable-pcre" if build.without? "pcre"
    args << "--with-metalink" if build.with? "libmetalink"
    args << "--with-gpgme-prefix=#{Formula["gpgme"].opt_prefix}" if build.with? "gpgme"

    system "./bootstrap" if build.head?
    system "./configure", *args
    system "make", "install"
  end

  test do
    system bin/"wget", "-O", "/dev/null", "https://google.com"
  end
end

</pre>

You see the install method definition as well as the desc, homepage and other properties. These define the output when we run brew commands using the wget formula.

Let's explore some of these commands and see what's going on in more detail
