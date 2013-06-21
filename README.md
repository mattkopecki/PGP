To contact me securely, copy my public key (the file publickey.asc, above) by either cloning the repository or downloading the <a href="https://github.com/mattkopecki/PGP/archive/master.zip">zip.</a> The key should also be available on PGP key servers and searchable by my name or email.

For those unfimiliar with PGP or looking for instructions to get set up themselves, take a look at <a href="http://www.robertsosinski.com/2008/02/18/working-with-pgp-and-mac-os-x/">this tutorial</a> written by Robert Sosinski, which I have reproduced below:

<h2>Working with PGP and Mac OS X</h2>


<p><span class="caps">PGP</span>, or Pretty Good Privacy, is a commonly used and very secure encryption program using public key cryptography.  Through <span class="caps">PGP</span>, you can encrypt information such as messages, documents and files in a manner so that only the recipient can decrypt and open them.</p>
<p>The goal of this tutorial is to get you up and running with <span class="caps">PGP</span> through terminal and familiar with its operation.</p>
<h3>An Encryption Primer</h3>
<p>There are two types of encryption, symmetric and asymmetric (also called public key).  Symmetric encryption uses one key to encrypt and decrypt messages.  Often, symmetric encryption works out very well as there are extremely strong algorithms available that secure data very quickly.  However, symmetric encryption carries the major drawback that a key must be transported securely to the recipient.  In regards to communicating through the internet, this is not really feasible.</p>
<p>Asymmetric encryption however, uses two keys, a public and private one.  When using asymmetric encryption, you offer your public key to everyone while keeping your private key secret. Messages that are encrypted with your public key can only be decrypted with your private key.  Because of this, you can send and receive encrypted messages with another party without having to transport an encryption key through secure means.</p>
<p>The other benefit of asymmetric encryption is that key pairs can be used to authenticate messages too.  This is done by encrypting information with your private key.  Upon receiving a message, the recipient will use your public key to decrypt it.  Encrypting any information with a private key for authentication purposes is referred to as creating a digital signature.</p>
<p>For example.  I have my public key posted <a href="/publickey.asc">here</a> as well on multiple public key servers.  Whenever someone wants to send me an encrypted message, they use my public key to encrypt it and their private key to sign it.  Once this is done, the only way to decrypt the message is to use my private key, which I keep heavily secured.  Upon receiving the encrypted message, I decrypt it with my private key and authenticate it with the senders public key.</p>
<p>Easy enough, lets get started.</p>
<h3>Installing the Software</h3>
<p><strong>1.</strong> <span class="caps">PGP</span> comes in many implementations, however the one you will be using in this tutorial is <span class="caps">GPG</span> (<span class="caps">GNU</span> Privacy Guard) as it follows the OpenPGP standard, is completely free and offers an easy to use Mac OS X installer.</p>
<p>Get the Mac <span class="caps">GPG</span> installer for your version of Mac OS X from <a href="http://macgpg.sourceforge.net">sourceforge</a>.</p>
<blockquote>
<p><strong>What are <span class="caps">PGP</span> Implementations?</strong></p>
<p><span class="caps">PGP</span> was at one time a commercial product by <span class="caps">PGP</span> Incorporated.  However, realizing that a strong open source encryption mechanism was so important, the encryption community and <span class="caps">PGP</span> Inc. worked together to make the OpenPGP standard.  Through OpenPGP, compatible variations of <span class="caps">PGP</span> could be created by third parties and distributed freely.</p>
<p>Learn more about OpenPGP <a href="http://en.wikipedia.org/wiki/Pretty_Good_Privacy#OpenPGP">here</a>.</p>
</blockquote>
<p><strong>2.</strong> Get Paranoid (optional)!  You would not be reading this if you trusted everyone, so stay in that frame of mind.  The next step is to verify the MD5 checksum for the file you just downloaded.  This way, you are sure the installer came from Mac <span class="caps">GPG</span> and not a man in the middle.  Navigate your terminal to the downloaded image and type the following (where the file name and MD5 checksum match for your particular version):</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>md5 GnuPG1.4.8.dmg | grep db046fd96e274dfe3c7021047561fb5a
MD5 <span class="o">(</span>GnuPG1.4.8.dmg<span class="o">)</span> <span class="o">=</span> db046fd96e274dfe3c7021047561fb5a
</code></pre>
</div>
<blockquote>
<p><strong>What did I just do?</strong></p>
<p>Files can be digested to create a string relatively unique to them, and one such digestion algorithm is MD5 (Machine Digest 5).  Next to the download link for the installer on Mac GPG&#8217;s webpage , you will also see a 32 character string, this is the MD5 digest for the disk image available for download.</p>
<p>In terminal, we use the md5 command to digest GnuPG1.4.8.dmg.  We then grep it for the MD5 string posted on Mac GPG&#8217;s website.  If it matches, it will echo the value.  If not, it will return a blank.</p>
<p>Learn more about MD5 <a href="http://en.wikipedia.org/wiki/MD5">here</a>.</p>
</blockquote>
<p><strong>3.</strong> Now that we know the disk image is legitimate, we can mount it and install Mac <span class="caps">GPG</span>.  This is a simple process as everything is done through an installation wizard.  Just follow the steps and you will be up and running in no time.</p>
<h3>Generating a Key</h3>
<p>Now that you have <span class="caps">GPG</span> installed, you will start generating your key pair and adding it to your key chain.  All of this will be done through the shell, so open a terminal and get ready for some typing.</p>
<p><strong>1.</strong> Start the process for generating your key pair.  Upon doing so, you will be asked to choose between three key types, pick the default <span class="caps">DSA</span> and Elgamal type by pressing enter.</p>
<p><strong><span class="caps">NOTE</span>: As this is your first time generating keys, <span class="caps">GPG</span> will warn you about creating a ~/.gnupg directory and exit.  Just run the command again and you will be good to go.</strong></p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --gen-key
gpg <span class="o">(</span>GnuPG<span class="o">)</span> 1.4.8; Copyright <span class="o">(</span>C<span class="o">)</span> 2007 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please <span class="k">select </span>what kind of key you want:
   <span class="o">(</span>1<span class="o">)</span> DSA and Elgamal <span class="o">(</span>default<span class="o">)</span>
   <span class="o">(</span>2<span class="o">)</span> DSA <span class="o">(</span>sign only<span class="o">)</span>
   <span class="o">(</span>5<span class="o">)</span> RSA <span class="o">(</span>sign only<span class="o">)</span>
Your selection?
</code></pre>
</div>
<blockquote>
<p><strong>What is <span class="caps">DSA</span> and Elgamal?</strong></p>
<p><span class="caps">DSA</span> (Digital Signature Algorithm) and Elgamal (Also called <span class="caps">ELG</span>) are the encryption algorithms used by <span class="caps">GPG</span>.  <span class="caps">DSA</span>, as the name suggests, is used for authentication while Elgamal is used for encrypting data.</p>
<p>Learn more about <span class="caps">DSA</span> <a href="http://en.wikipedia.org/wiki/Digital_Signature_Algorithm">here</a> and Elgamal <a href="http://en.wikipedia.org/wiki/Elgamal">here</a>.</p>
</blockquote>
<p><strong>2.</strong> Choose your Elgamal encryption (<span class="caps">ELG</span>-E) key size. <span class="caps">GPG</span> uses 1024 bit keys for  <span class="caps">DSA</span>, however allows you to choose your Elgamal key length.  The default 2048 bit length should be plenty, so press enter.</p>
<div class="highlight"><pre><code class="bash">DSA keypair will have 1024 bits.
ELG-E keys may be between 1024 and 4096 bits long.
What keysize <span class="k">do </span>you want? <span class="o">(</span>2048<span class="o">)</span>
</code></pre>
</div>
<p><strong>3.</strong> Choose your key pair&#8217;s expiration date. Unless you are really paranoid, the default non-expiring key should be fine.  Press enter.</p>
<div class="highlight"><pre><code class="bash">Please specify how long the key should be valid.
         <span class="nv">0</span> <span class="o">=</span> key does not expire
      &lt;n&gt;  <span class="o">=</span> key expires in n days
      &lt;n&gt;w <span class="o">=</span> key expires in n weeks
      &lt;n&gt;m <span class="o">=</span> key expires in n months
      &lt;n&gt;y <span class="o">=</span> key expires in n years
Key is valid <span class="k">for</span>? <span class="o">(</span>0<span class="o">)</span>
</code></pre>
</div>
<p><strong>4.</strong> Confirm that your key will not expire by typing &#8220;y&#8221; and pressing enter.</p>
<div class="highlight"><pre><code class="bash">Key does not expire at all
Is this correct? <span class="o">(</span>y/N<span class="o">)</span> y
</code></pre>
</div>
<p><strong>5.</strong> Create an identity for your key.  There are three parts, your name, email address and comment.  The comment for your key is optional, and is mostly used when you choose to have more then one key pair to keep track of. Fill in your information, type &#8220;O&#8221; and press enter.</p>
<div class="highlight"><pre><code class="bash">You need a user ID to identify your key; the software constructs the user ID
from the Real Name, Comment and Email Address in this form:
    <span class="s2">&quot;Heinrich Heine (Der Dichter) &lt;heinrichh@duesseldorf.de&gt;&quot;</span>

Real name: First and Last Name
Email address: name@domain.com
Comment: Mac GPG Tutorial
You selected this USER-ID:
    <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>

Change <span class="o">(</span>N<span class="o">)</span>ame, <span class="o">(</span>C<span class="o">)</span>omment, <span class="o">(</span>E<span class="o">)</span>mail or <span class="o">(</span>O<span class="o">)</span>kay/<span class="o">(</span>Q<span class="o">)</span>uit? O
</code></pre>
</div>
<p><strong>6.</strong> Choose your passphrase.  In order to keep your private key secure (else someone could open your computer and read it), <span class="caps">PGP</span> will encrypt it with symmetric encryption.  The passphrase you are about to give will be used as the encryption key.  Everytime you want to use your private key, <span class="caps">PGP</span> will ask for your passphrase. If it is correct, <span class="caps">PGP</span> will decrypt your private key and use it.  Choose something long (hence the term pass <strong>phrase</strong>) and press enter.  You will then be asked to confirm your passphrase.  Do so and press enter again.</p>
<blockquote>
<p><strong>What if I forget my passphrase?</strong></p>
<p>If passphrases were easily cracked, they would not be all that useful.  As such, your only real option is to make a new key pair.</p>
<p>Later in this tutorial, you will go through the process of making a revocation certificate, so you can &#8220;turn off&#8221; your public key if this were to happen.</p>
<p>Learn more about passphrases <a href="http://en.wikipedia.org/wiki/Passphrase#Compared_to_passwords">here</a>.</p>
</blockquote>
<p><strong>7.</strong> <span class="caps">GPG</span> will then start to print some gibberish while it generates your key pair.  During this period, <span class="caps">GPG</span> will also ask you to do something with your computer such as typing on the keyboard, moving your mouse or using your hard disk drive.  This serves the purpose of creating more entropy and helps in generating a better key.</p>
<blockquote>
<p><strong>What is entropy and why does it generate a better key?</strong></p>
<p>In computing terms, entropy is randomness.  Computers are actually not that good at making random decisions, however people are.  By moving the mouse and pressing keys, you are making your computer &#8220;randomly&#8221; process instructions and thus, generate a stronger key pair.</p>
<p>Randomness is very important for generating encryption keys.  If there were a pattern for creating encryption keys, then there would be a reversed pattern for creating decryption keys, which would be a big problem.  A lot of work has gone into making random number generators for encryption systems, but banging on the keyboard is probably as random as you can get.  At least for the moment.</p>
<p>Learn more about entropy <a href="http://en.wikipedia.org/wiki/Key_%28cryptography%29#Key_choice">here</a> and random number generation <a href="http://en.wikipedia.org/wiki/Applications_of_randomness#Cryptography">here</a>.</p>
</blockquote>
<p><strong>8.</strong> After your key pair is generated, <span class="caps">GPG</span> will then print the following information to the screen.  The basic gist is that <span class="caps">GPG</span> does the following:</p>
<ol>
    <li>Creates a trust database called ~/.gnupg/trustdb.gpg. Within this database, <span class="caps">GPG</span> will store how well you trust public keys you receive from others.</li>
	<li>Marks your public key with the highest level of trust (ultimately), as it assumes that you can completely trust any key you personally generate.</li>
	<li>Reports that a public and secret (private) key are created and authenticated (signed) to you.</li>
	<li>Goes through the motions of how it assigned trust.  Assigning trust is a bit beyond this tutorial, however you can learn more <a href="http://www.gnupg.org/gph/en/manual.html#AEN335">here</a> and <a href="http://en.wikipedia.org/wiki/Web_of_trust">here</a>.</li>
	<li>Prints information about the newly created public key including its fingerprint and subkey (to be discussed soon).</li>
</ol>
<div class="highlight"><pre><code class="bash">gpg: ~/.gnupg/trustdb.gpg: trustdb created
gpg: key A7327C0E marked as ultimately trusted
public and secret key created and signed.

gpg: checking the trustdb
gpg: 3 marginal<span class="o">(</span>s<span class="o">)</span> needed, 1 <span class="nb">complete</span><span class="o">(</span>s<span class="o">)</span> needed, PGP trust model
gpg: depth: 0  valid:   2  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 2u
pub   1024D/A7327C0E 2008-02-14
      Key <span class="nv">fingerprint</span> <span class="o">=</span> 2291 60E9 A962 5C22 6E4F  1F20 120A 2881 A732 7C0E
uid                  First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;
sub   2048g/60ED8082 2008-02-14
</code></pre>
</div>
<p><strong>9.</strong> List the public keys in your key chain (in which you should only have one at this time, yours).  Take special note of your key id (A7327C0E in this example) as you will need it later.  You can always list your keys again in the future if needed.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --list-keys
~/.gnupg/pubring.gpg
--------------------------------
pub   1024D/A7327C0E 2008-02-14
uid                  First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;
sub   2048g/60ED8082 2008-02-14
</code></pre>
</div>
<p><strong>10.</strong> Finally, go ahead and list the private keys in your key chain too. (in which you should also only have one).</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --list-secret-keys
~/.gnupg/secring.gpg
--------------------------------
sec   1024D/A7327C0E 2008-02-14
uid                  First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;
ssb   2048g/60ED8082 2008-02-14
</code></pre>
</div>
<h3>Managing Your Keys</h3>
<p>Now that you have a key pair, the next step is managing your keys properly.  You will do this by sharing your public key with others and securely backing up your private key.</p>
<p><strong>1.</strong> Create a directory in your home called keymat, so you have a place to work in for this tutorial.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span><span class="nb">cd</span>
<span class="nv">$ </span>mkdir keymat
<span class="nv">$ </span><span class="nb">cd </span>keymat
</code></pre>
</div>
<p><strong>2.</strong> Output your public key as an <span class="caps">ASCII</span> armored file (replacing name@domain.com with the email address for your key).</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg -ao publickey.asc --export name@domain.com
</code></pre>
</div>
<blockquote>
<p><strong>What is <span class="caps">ASCII</span> armor?</strong></p>
<p><span class="caps">ASCII</span> armor is a term used with <span class="caps">PGP</span> for encoding binary data into text.  Although this &#8220;armor&#8221; does not make your data any more secure against brute-force attacks, it does protect your data when sent over a series of communication paths.</p>
<p>For example, if  you wanted to send your public key through email, mail servers and spam filters may not be able to process your key in binary format and as such, throw it out.  With <span class="caps">ASCII</span> armor however, it is treated as normal text and can survive its fight through the internet.</p>
<p><span class="caps">PGP</span> can encode any encrypted file with <span class="caps">ASCII</span> armor which keeps your transmission options open.</p>
<p>Learn more about <span class="caps">ASCII</span> armor <a href="http://en.wikipedia.org/wiki/ASCII_armor">here</a>.</p>
</blockquote>
<p><strong>3.</strong> After a short wait,  you should have a publickey.asc file in  your keymat directory. Take a look at it.</p>
<div class="highlight"><pre><code class="bash">-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1.4.8 <span class="o">(</span>Darwin<span class="o">)</span>

mQGiBEe0cB0RBADScWGLrplJfLUrPBFNrBUqr8nh8AvTRcFaMt5ODK7c5G9JqNZd
oDSI9WcbDVl26bleda7p0ZGTraY8Dhg58JQa140Rjfq2dMNq9/Dlayl5WHEWhX2L
WhXz6HewOiI6T1ikDj9NBjLN72QYL/W57Zrf65IMvJxSjQEyu1BGPmbzfwCgkEC6
LWygY2KDliAwKQB7klj26VsEAMTdGqr1ObGwx4mu3t9Jej0+vcLBvNfKgoSB/Ewx
fShUNXkvxHk6yleaLKe8TlPERfhkxCj8A32AN+TJbjcwgka0gOXwCP0e2N8+niRK
RXkcooWRn7OOJ7Evw7dgQ722TCXJ+YsJya7mJ0oi61zsbQojrNk+7Y5P+jddm3tm
RHgLA/0TY4bLMK3p83As/mF3SSp8gsFTnf9QO2tsVVHrNDUpmNm3AcpLNyO5z8AX
C2vpuPlAjgQNLvOxCjZ2mSRq8329bnmMkoAs8u2rrBi0fVJ2FvvxLiA5CgqF2rWA
FKkWa2MZUUwOvlXzGzHSCJsIRYgRlMuPH6t8D7U1+sVrHfs9S7Q4Rmlyc3QgYW5k
IExhc3QgTmFtZSAoTWFjIEdQRyBUdXRvcmlhbCkgPG5hbWVAZG9tYWluLmNvbT6I
YAQTEQIAIAUCR7RwHQIbAwYLCQgHAwIEFQIIAwQWAgMBAh4BAheAAAoJEBIKKIGn
MnwO+dYAnierWBzFSDIzHAH9umnSf8sUXTeSAKCMhf0eN9uSblNdgHwt3BvQVO7J
kLkCDQRHtHAdEAgA/x8rqhTDqSOCl+csy6zHDQoxjmC3mOoBhvMik+ZB96qS20pC
U0eV6QYgoPhZ/wkPsXFO9EHJrrJsfLnzRxH7ylw0TmMHnVrN3GFRdhuRhYyO8sKp
N9eKyWQpFtOry3JXYNpsFcUqOfutZ3v5xcUJ+r2cE11/VMmQykFiKyy/dAlD2nqs
hRhubN5xIE0Gu/0VZw4LQDO9jYZSEoX4QkntL8h2U85M1lmuWszUsvK5SV/OH57X
hQYD4syR/QogIVNHHhS4/qFFnhX7oXggD73kbF+YRwgqvhbNyhsjQ7o0Rhyfq+XH
k8/22/NDOx6YD3KmBKBulldv5gf/oALFBcnVXwADBwf9GlNviLWXmsVV5fpvqsuT
gsX6vvdNpTGIksCviyptjT/Pf8ZS4u7pQuQ37sJ9LHQ1ld2tloAYf1meee+bZzTO
zX4+4iohUxvGdGn+pBSDSS2Cs9cztl/gyz4rPga9g//Z2O0F8+1kP1RUBaFRL4lm
avowgb9bzsWaax+tLzB2QwFgk0Phe5lULIKDpBOVk2OQB2L3MeRZoykN4W/IKaYP
/c4kYN6IGVBrgdGE7nnO/ZCyvqNQq6ClIvWtrJdwiOEZTGXka06LW3+O3DHNOsjC
uZAtMUx+pzkU0rNg+x0wPXYJOuN7Y3QuxD3gKrtPI2W7Ho13iVXuXlYfhZlaZotu
bIhJBBgRAgAJBQJHtHAdAhsMAAoJEBIKKIGnMnwOtacAnjYM5hAl6Zcn359Jy7RE
<span class="nv">Apx7kRj2AJ9AmMJBTDFUTj6y8HDMzwtmz3Aaug</span><span class="o">==</span>
<span class="o">=</span>pyBn
-----END PGP PUBLIC KEY BLOCK-----
</code></pre>
</div>
<p><strong>4.</strong> There it is, your public key.  You can now upload it to your website, give it to your friends, email it to your coworkers, print it out and post it on your front door.  This is what people need to contact you, and only you, securely.</p>
<p><strong>5.</strong> Next, send your public key to a key server, so that people interested in contacting you securely have an easy way to access it. For this, you will need your key&#8217;s id (replacing A7327C0E with the id for your key).</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --send-keys A7327C0E
gpg: sending key A7327C0E to hkp server subkeys.pgp.net
</code></pre>
</div>
<p><strong>6.</strong> Good to go. Once your key is propagated to a series of <span class="caps">PGP</span> key servers, anyone can go to a key lookup website, type the key&#8217;s information (key id, owners name or email address) and get it.  Two examples of key lookup sites are <a href="http://keyserver.pgp.com">keyserver.pgp.com</a> and <a href="http://pgp.mit.edu">pgp.mit.edu</a>.  Check them out and see if anyone you know has a public key available.</p>
<blockquote>
<p><strong>How do key servers work?</strong></p>
<p>There are many <span class="caps">PGP</span> key servers throughout the world, however you only need to send your public key to one, the default for <span class="caps">GPG</span> being subkeys.pgp.net.  After one key server receives your key, it <strong>should</strong> propagate it to many other key servers, some of which having handy key lookup websites.</p>
<p>Learn more about key servers <a href="http://en.wikipedia.org/wiki/Key_server_%28cryptographic%29">here</a>.</p>
</blockquote>
<p><strong>7.</strong> Once your key is propagated throughout a series of key servers, you can only have it disabled by generating and uploading a revocation certificate.  This is very useful if your private key were to become compromised.  There is a catch 22 however.  If you want to revoke it because you forget your passphrase, you will need to generate a revocation certificate. But to generate a revocation certificate, you will need your passphrase. As such, a good practice is to pre-emptively generate a revocation certificate so you have it if this ever happens. Do this now (replacing A7327C0E with the id for your key).</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg -ao revokecert.asc --gen-revoke A7327C0E

sec  1024D/A7327C0E 2008-02-14 First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;

Create a revocation certificate <span class="k">for </span>this key? <span class="o">(</span>y/N<span class="o">)</span> y
Please <span class="k">select </span>the reason <span class="k">for </span>the revocation:
  <span class="nv">0</span> <span class="o">=</span> No reason specified
  <span class="nv">1</span> <span class="o">=</span> Key has been compromised
  <span class="nv">2</span> <span class="o">=</span> Key is superseded
  <span class="nv">3</span> <span class="o">=</span> Key is no longer used
  <span class="nv">Q</span> <span class="o">=</span> Cancel
<span class="o">(</span>Probably you want to <span class="k">select </span>1 here<span class="o">)</span>
</code></pre>
</div>
<p><strong>8.</strong> <span class="caps">GPG</span> will ask the reason for generating a revocation certificate, as your key is not compromised, superseded and is still in use, just give no reason by typing &#8220;0&#8221; and pressing enter.</p>
<div class="highlight"><pre><code class="bash">Your decision? 0
</code></pre>
</div>
<p><strong>9.</strong> Next, give a description why you are making this revocation certificate.  I would just be honest on this one.</p>
<div class="highlight"><pre><code class="bash">Enter an optional description; end it with an empty line:
&gt; In <span class="k">case </span>I forget my passphrase.
&gt;
</code></pre>
</div>
<p><strong>10.</strong> <span class="caps">GPG</span> will then echo your reason for revocation and ask if you are sure.  Type &#8220;y&#8221; and press enter.  You will then be prompted for you passphrase.  Type it in and press enter.</p>
<div class="highlight"><pre><code class="bash">Reason <span class="k">for </span>revocation: No reason specified
In <span class="k">case </span>I forget my passphrase.
Is this okay? <span class="o">(</span>y/N<span class="o">)</span> y

You need a passphrase to unlock the secret key <span class="k">for</span>
user: <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
1024-bit DSA key, ID A7327C0E, created 2008-02-14

Revocation certificate created.

Please move it to a medium which you can hide away; <span class="k">if </span>Mallory gets
access to this certificate he can use it to make your key unusable.
It is smart to print this certificate and store it away, just in <span class="k">case</span>
your media become unreadable.  But have some caution:  The print system of
your machine might store the data and make it available to others!
</code></pre>
</div>
<p><strong>11.</strong> After a short wait, you should have a revokecert.asc file in your keymat directory. Take a look at it.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$cat</span> revokecert.asc
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1.4.8 <span class="o">(</span>Darwin<span class="o">)</span>
Comment: A revocation certificate should follow

baKlTel7dNK/vvVkuLgWbRNE8vaikEbTdmPSYdpc9q828O3w51dDCjTQhHPh5JpX
yo7oxpJMANs/TdLzOtfcSdD9KgbTW7cdky7Htd9IbQnCAUoC1axK/GJpyMY89A5e
<span class="nv">LTMjufSJESOxdQ</span><span class="o">==</span>
<span class="o">=</span>b08k
-----END PGP PUBLIC KEY BLOCK-----
</code></pre>
</div>
<p><strong>12.</strong> You now have your public key accessible to others and your revocation certificate on your computer.  The next step is to make a backup of your private key.  This is very important, as if you were loose your private key, you will be unable to retrieve any information encrypted by your public key.  For security purposes, you will also symmetrically encrypt your private key backup with a passphrase and then output it as an <span class="caps">ASCII</span> armored file (replacing A7327C0E with the id for your key).</p>
<div class="highlight"><pre><code class="bash">gpg -a --export-secret-keys A7327C0E | gpg -aco privatekey.pgp.asc
</code></pre>
</div>
<p><strong>13.</strong> After entering your passphrase twice, you will now have a privatekey.pgp.asc file in your keymat directory.  Take a look at it.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>cat privatekey.pgp.asc 
-----BEGIN PGP MESSAGE-----
Version: GnuPG v1.4.8 <span class="o">(</span>Darwin<span class="o">)</span>

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
<span class="o">=</span>tKwz
-----END PGP MESSAGE-----
</code></pre>
</div>
<p><strong>14.</strong> Good to go, your private key is backed up as an encrypted file, your public key is available to others and you have a contingency plan if you forget your passphrase. Now, you should save all three of these files (publickey.asc, privatekey.pgp.asc and revokecert.asc) in a safe place.  What I do is have them burned onto a cd, printed on paper and stored in a safe within my house. After all three files are backed up, you should then destroy the private key backup and revocation certificate on your computer (I recommend the Mac OS X Secure Empty Trash feature).</p>
<h3>Importing and Verifying Public Keys</h3>
<p>Your public key is now available throughout the internet and people interested in sending you an encrypted message have easy access to it.  The next step is to go through the motions of importing a public key and verifying its authenticity.</p>
<p><span class="caps">PGP</span> also offers an easy way to verify public keys as every one also has a 40 character &#8220;fingerprint&#8221;.  As a fingerprint is much shorter then an <span class="caps">ASCII</span> armored public key and has only 16 possible characters instead of 64, it is much easier to memorize and confirm in a voice conversation. Lets see how all this works.</p>
<p><strong>1.</strong> Once you have downloaded a public key from a friend or co-worker, you can add it to your key chain by doing the following:</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --import publickey.asc
gpg: key A7327C0E: <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
gpg: Total number processed: 1
gpg:               imported: 1
</code></pre>
</div>
<p><strong><span class="caps">NOTE</span>: Importing your own public key is not neccessary, however we will do so in this tutorial just so you can go through the motions and become familiar with the proccess.</strong></p>
<p><strong>2.</strong> Importing a file is one way to get a public key into your key chain, however there is an easier way.  You already uploaded your key to a public key server so that others can easily get a hold of it.  Now, try getting the key used in this tutorial by searching for it. For this tutorial, you will use the key&#8217;s unique id, however entering a name or email address will make the server perform a search and return any relevant results.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --search-keys A7327C0E
gpg: searching <span class="k">for</span> <span class="s2">&quot;A7327C0E&quot;</span> from hkp server subkeys.pgp.net
<span class="o">(</span>1<span class="o">)</span>     First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;
          1024 bit DSA key A7327C0E, created: 2008-02-14
Enter number<span class="o">(</span>s<span class="o">)</span>, N<span class="o">)</span>ext, or Q<span class="o">)</span>uit &gt;
</code></pre>
</div>
<p><strong>3.</strong> There it is.  From here, you can type &#8220;1&#8221; and press enter.  Also, keep note of the key&#8217;s id (A7327C0E) as you will need it very soon.</p>
<div class="highlight"><pre><code class="bash">gpg: requesting key A7327C0E from hkp server subkeys.pgp.net
gpg: key A7327C0E: <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
gpg: Total number processed: 1
gpg:               imported: 1
</code></pre>
</div>
<p><strong>4.</strong> You now know how to import keys from files and key servers. Next, you will generate and use the key&#8217;s fingerprint to verify it with the owner through another channel.  Try it by verifying the Mac <span class="caps">GPG</span> Tutorial key with its fingerprint.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --fingerprint A7327C0E
pub   1024D/A7327C0E 2008-02-14
      Key <span class="nv">fingerprint</span> <span class="o">=</span> 2291 60E9 A962 5C22 6E4F  1F20 120A 2881 A732 7C0E
uid                  First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;
sub   2048g/60ED8082 2008-02-14
</code></pre>
</div>
<blockquote>
<p><strong>How are <span class="caps">PGP</span> Fingerprints Created</strong></p>
<p>Fingerprints are created by digesting your public key with SHA1 (Secure Hashing Algorithm 1), a digestion algorithm similar to the MD5 algorithm talked about earlier. Although a SHA1 digest of your public key cannot be anywhere near as unique as your public key, mathematics are on your side in terms of being unique to any fingerprint an imposer could create through <span class="caps">PGP</span>.</p>
<p>Learn more about <span class="caps">SHA</span> <a href="http://en.wikipedia.org/wiki/SHA_hash_functions">here</a>.</p>
</blockquote>
<p><strong>5.</strong> <span class="caps">GPG</span> makes everything easy for you as it adds a space every four characters and a double space in the middle of the string. If the fingerprint for the key you have matches what your friend or co-worker say, then it should be legitimate.  For the Mac <span class="caps">GPG</span> Tutorial key, match the fingerprint with what the example shows above and if it checks out, feel free to sign it.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg --sign-key A7327C0E

pub  1024D/A7327C0E  created: 2008-02-14  expires: never       usage: SC
sub  2048g/60ED8082  created: 2008-02-14  expires: never       usage: E
<span class="o">(</span>1<span class="o">)</span>. First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;

pub  1024D/A7327C0E  created: 2008-02-14  expires: never       usage: SC
Primary key fingerprint: 2291 60E9 A962 5C22 6E4F  1F20 120A 2881 A732 7C0E

     First and Last Name <span class="o">(</span>Mac GPG Tutorial<span class="o">)</span> &lt;name@domain.com&gt;

Are you sure that you want to sign this key with your
key <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span> <span class="o">(</span>A7327C0E<span class="o">)</span>

Really sign? <span class="o">(</span>y/N<span class="o">)</span>
</code></pre>
</div>
<p><strong>6.</strong> <span class="caps">GPG</span> will then ask you if you are sure, type &#8220;y&#8221; and press enter.  Then, verify your passphrase in order to sign this key.  The reason why you need your passphrase is because your ~/.gnupg/trustdb file is also encrypted to ensure that malicious users do not assign levels of trust to keys for you.  Enter your passphrase and press enter.</p>
<div class="highlight"><pre><code class="bash">You need a passphrase to unlock the secret key <span class="k">for</span>
user: <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
1024-bit DSA key, ID A7327C0E, created 2008-02-13
</code></pre>
</div>
<p><strong>7.</strong> You now have a signed public key in your key chain.  Signing keys you trust is always a good practice as anyone can import keys into your key chain, but only you can assign trust to them.  Be very weary of using any public keys that you have not signed and always try to verify any new keys with their owners by using their fingerprint.</p>
<h3>Encrypting and Decrypting Messages</h3>
<p>Now that you are setup and familiar with <span class="caps">GPG</span>, we can now start using it to encrypt and decrypt messages. In order to keep this part of the tutorial simple, you will only be sharing information with yourself.  Once you get the hang of it, you can then start sending encrypted messages to any other <span class="caps">PGP</span> user.</p>
<p><strong>1.</strong> Make a new directory in your home called crypto, so you have a place to work in for this tutorial.</p>
<div class="highlight"><pre><code class="bash"><span class="nb">cd</span>
mkdir crypto
<span class="nb">cd </span>crypto
</code></pre>
</div>
<p><strong>2.</strong> Make a new plain text file using your favorite text editor, type a message and save it in your crypto directory as plain.txt. I wrote the following:</p>
<div class="highlight"><pre><code class="bash">This is my secret message to myself.
</code></pre>
</div>
<p><strong>3.</strong> Now, encrypt this file with your public key and sign it with your private key.  This will facilitate that you are sending the file to yourself, and ensuring it came from yourself.  You will also output the ciphered text with <span class="caps">ASCII</span> armor into cipher.asc.  As you are using your private key to digitally sign this message, you will be asked for your passphrase.  Just type it in and press enter (replacing name@domain.com with the email address for your key).</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg -ao cipher.asc -esr name@domain.com plain.txt 

You need a passphrase to unlock the secret key <span class="k">for</span>
user: <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
1024-bit DSA key, ID A7327C0E, created 2008-02-13
</code></pre>
</div>
<blockquote>
<p><strong><span class="caps">DSA</span> for authentication and Elgamel for encryption</strong></p>
<p>When you generated your key pair, you noticed that you actually created two keys, one 1024 bit <span class="caps">DSA</span> key and one 2048 bit Elgamel sub key.  As you are encrypting and signing your message, <span class="caps">GPG</span> will use your <span class="caps">DSA</span> private key to sign the message and your Elgamel public sub key to encrypt it. When you decrypt this message, the process is reversed by using your Elgamel private sub key for decryption and <span class="caps">DSA</span> public key for authentication.  Currently, all <span class="caps">PGP</span> implementations which adhere to the OpenPGP standard (like <span class="caps">GPG</span>) use these two cryptographic algorithms.</p>
</blockquote>
<p><strong>4.</strong> After a short wait, you should now have a cipher.asc file in your crypto directory.  Take a look.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>cat cipher.asc
-----BEGIN PGP MESSAGE-----
Version: GnuPG v1.4.8 <span class="o">(</span>Darwin<span class="o">)</span>

hQIOAwqfNQVg7YCCEAgAr8xgbfhektMMwlnT4OaHHQxvNnn+2q6jxQDGLEDTcNxu
A9uiNyMs0UzRjRRbYUyRucaBjnZGPZiv1qtaqWaDv4Aw3M9SWSnV17K7GqDpUq+6
7SmoO+4wUyLFvYo/5dWgUhU1OE4IVGF7A0dWVSYScHuO0tH+KbdpV+fCCOZ5dY0M
MNgAaLt3Llv3P9uJOweVYJD/+++BdMUBrvPpURHmQPj1sVyo+994O9LBnW7jxuOQ
oNhNJY+vJZ/ZBkdwr/uUTtzrjQgvGz8bLJjCKZRbDsMKS9KQHJvkYN5b2tUidufL
GOJ3f1/TQDtJVtwObmLV4rf5N08jdpd3dn2rk1H9Sgf+LpHJMew1U+cw0bg7zBe5
ow28Hzh1caJqqs1pACC9HpecSCRcSKvsYbEWH7vN4/psOH/2Kegm69BKnPsqMJr/
29r+wk+X/PLB6HdfYhGge09Ss+Ks1HJ1Jay+GDek5aHXXiZjtbgBrEF+aKf3K2lG
eOlks19s3C1MiDRzC1vsIDx8eD3q6IBuWQvqWylDmtgs73/AGLSKiOva/8WNdfJ+
pPgiUcjZXwEl5Cl3mOzkdobVOkvOwuheBoRUyb6lP8IHafbX5I5Kqjkk20SjIqRL
tbcghyVIa9Q0RYgzSI0c2Al/prA+83hJM//Jw4RFS8XawPBQK4xg2VUgfVVAdpDz
ZNJlAYpVQ9c7G66OVLbIEa/5/aeMw5mOqc6yANQNp3T1x36mJ3oH8T+2N4d6Qnn1
eVbi59V/ohmztX7/c79SApnHGojRFh8UlBX2ZRt2QS/96Q2AvvUHx7PkMfIS+kwT
Z+9UgxOZbr0<span class="o">=</span>
<span class="o">=</span>u09w
-----END PGP MESSAGE-----
</code></pre>
</div>
<p><strong>5.</strong> Good to go, you now have an encrypted message that you can send secretly to, well, yourself.  Lets decrypt it into a file called decrypted_plain.txt and see what we get back. You will need to type your passphrase for this too, as you are using your private key to decrypt cipher.asc.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>gpg -o decrypted_plain.txt -d cipher.asc 

You need a passphrase to unlock the secret key <span class="k">for</span>
user: <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
2048-bit ELG-E key, ID 60ED8082, created 2008-02-14 <span class="o">(</span>main key ID A7327C0E<span class="o">)</span>

gpg: encrypted with 2048-bit ELG-E key, ID 60ED8082, created 2008-02-14
      <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
File <span class="sb">`</span>decrypted_plain.txt<span class="err">&#39;</span> exists. Overwrite? <span class="o">(</span>y/N<span class="o">)</span> y
gpg: Signature made Fri Feb 15 13:07:19 2008 EST using DSA key ID A7327C0E
gpg: Good signature from <span class="s2">&quot;First and Last Name (Mac GPG Tutorial) &lt;name@domain.com&gt;&quot;</span>
</code></pre>
</div>
<p><strong>6.</strong> <span class="caps">GPG</span> will now decrypt the file and also let you know that the signature is good. Now, take a look at your decrypted_plain.txt file and see if everything went well.</p>
<div class="highlight"><pre><code class="bash"><span class="nv">$ </span>cat decrypted_plain.txt
This is my secret message to myself.
</code></pre>
</div>
<p><strong>7.</strong> Fantastic, you just sent your first secure message to yourself. That&#8217;s all there is to it.</p>
<blockquote>
<p><strong>What else can I encrypt and decrypt?</strong></p>
<p>You can use <span class="caps">GPG</span> for more then encrypting short messages.  Binary files, source code, Word documents, pictures, video, anything can be encrypted to keep it from prying eyes.  The format which you choose to output depends on how you wish to send the information.  A good rule of thumb is to output encrypted plain text (which would include markup like <span class="caps">HTML</span> and source code) with <span class="caps">ASCII</span> armor and output encrypted binary as binary (by not using the -a option).  However, if you wanted to send a picture to someone via email or some other messaging system (even printing it out and sending it via snail mail), then <span class="caps">ASCII</span> armor would be the best output to do it in.</p>
<p>Keep in mind, you can encrypt entire directories too. All you have to do is zip or tar the directory and encrypt the archive.  This is especially handy if you want to backup sensitive documents for yourself, as you can encrypt the archive with your public key and store them anywhere (even GMail or Amazon S3) without worry.</p>
</blockquote>
<h3>Conclusion</h3>
<p>You went through a lot information in this tutorial. However, you should now have a very good foundation on how cryptography works and some best practices on how to use it. The best way to incorporate cryptography into your life is to get into the habit of using it.  As such, get your friends, family and coworkers familiar with <span class="caps">PGP</span> too.  As time goes on, securing your information will be as second nature as running spell check before you send an email.</p>
</div>
      
      
Copyright &copy; 2012 Robert Sosinski
