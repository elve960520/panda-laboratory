## Linux
#### linux下使用md5校验文件完整性
``` bash
md5sum ./file.tar
```
#### linux下使用sha1校验文件完整性
``` bash
sha1sum ./file.tar
```
#### linux下使用sha256校验文件完整性
``` bash
sha256sum ./file.tar
```

## Windows
#### Windows下使用md5校验文件完整性
``` cmd
certutil -hashfile .\file.zip md5
```
#### Windows下使用sha1校验文件完整性
``` cmd
certutil -hashfile .\file.zip sha1
```
#### Windows下使用sha256校验文件完整性
``` cmd
certutil -hashfile .\file.zip sha256
```

## macOS
#### macO下使用md5校验文件
``` bash
md5 ./file.tar
```
#### macO下使用sha1校验文件
``` bash
shasum -a 1 ./file.tar
```
#### macO下使用sha256校验文件
```bash
shasum -a 256 ./file.tar
```