Proxychains is a very useful software to enforce a given proxy chain to a given software. Useful for reverse engineering HTTP communication of closed software by pushing them to use Burp.
# M1 problem solution
Stock proxychains-ng installed with Brew with oftentimes fail due to architecture discrepancies. In such cases, the error will look similar to the following:
```
Could no be loaded: tried /usr/local/lib/libproxychains4.dylib (mach-o file, but is an incompatible architecture (have arm64, need x86_64))
```
So the solution is to compile proxychains yourself for a wanted architecture:
```bash
# install rosetta so you can emulate intel architecture
sudo softwareupdate --install-rose
git clone git@github.com:rofl0r/proxychains-ng.git
cd proxychains-ng
CFLAGS="-arch x86_64" LDFLAGS="-arch x86_64" ./configure 
make
sudo make install
```
Depending on the software we're trying to wrap, the required architecture might chainge. In such case just rebuild and reinstall with the required one. So far the following were encountered:
- x86_64
- ARM64
- ARM64e

Some guys even tried with some successes mixing (for instance they first created libproxychains4.dylib with arm64e, then rest with regular arm64 and installed it like that)
# References
- [https://gist.github.com/pich4ya/f4ae0b04f30653b2bcc2f2246bb564af#comments](https://gist.github.com/pich4ya/f4ae0b04f30653b2bcc2f2246bb564af#comments)
- [https://github.com/rofl0r/proxychains-ng/issues/433](https://github.com/rofl0r/proxychains-ng/issues/433)