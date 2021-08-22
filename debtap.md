# Using debtap install .deb package to arch linux

1. Download deptap

```bash
sudo pacman -S deptap
#or
yay -S deptap
```

2. Update debtap source

```bash
sudo debtap -u
```

3. Transfer .deb file to arch pkg format .pkg.tar.zst

```bash
sudo debtap xxxxxx.deb
```

4. Install a local package that is not from a remote repository

```bash
sudo pacman -U your_.pkg.tar.zst_file_generated_by_step_3.pkg.tar.zst
```

5. Finished



