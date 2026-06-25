### Buiding a Package

1. Install build tools with: `sudo apt install build-essential debhelper devscripts`
2. Within the root of the project directory, run `debuild -us -uc` to build the deb package.
    - *The `-us` and `-uc` flags specify to build an unsigned deb package.*
3. You can clean/remove temporary build files with: `debian/rules clean`.