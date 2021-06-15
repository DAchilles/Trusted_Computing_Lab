# 0x00 brief
  HUST 可信计算实验，大部分代码来自课堂所给

# 0x01 编译：
1. 在此目录下运行make

# 0x02 初始化

1. sudo modprobe tpm_tis # 如果是tpm emulator则：sudo modprobe tpmd_dev; tpmd -f -d clear
2. 运行sudo tcsd
3. 确保TPM没有被TakeOwnership，否则会出错
4. 进入init目录，运行./Tspi_TPM_TakeOwnership01 -v 1.2
5. 运行 ./create_mig_key -v 1.2（输入pin）[k1]

# 0x03 KeyHierarchy

1. 进入Key hierarchy目录
2. 参考K1. K2. K3的创建过程，以及TSS文档，完善create_register_key.c中创建K4的代码，运行./create_register_key -v 1.2
3. 参考K1. K2. K3的加载过程，以及TSS文档，完善load_key.c中加载K4的代码，运行./load_key -v 1.2

# 0x04 Seal. Unseal和extend

1. 进入SealUnseal目录

2. 运行./seal -v 1.2  （成功）

3. 运行./unseal -v 1.2 （成功）

4. 运行./extend -v 1.2 （成功）

5. 运行./unseal -v 1.2  （失败）

6. 运行./seal_file test.c test.en（查看文件test.en的内容）

   > unseal_file.c 由同学们自己完成。

7. 运行./unseal_file test.en test.de（查看文件test.de的内容）

8. 运行./extend -v 1.2

9. 运行./unseal_file test.en test.de（失败）

# 0x05 KeyMigration

1. 进入Key Migration目录

   > platform_dst.c中的TODO部分由同学们自己来完成

2. 在机器1中运行./platform_dst -g ，会产生名为srk.pub的文件

3. 把文件srk.pub拷贝到机器2中

4. 在机器2中运行./platform_src ，会产生名为mig.blob的文件(k1)

5. 把文件srk.pub拷贝到机器1中

6. 在机器1中运行./platform_dst -m

# 0x06 RemoteAttestation（可选）
机器1：

1. 进入Remote Attestation\init目录
2. 运行./Create_AIK
3. 返回上级目录
4. 运行./RAServer

机器2：
1. 进入Remote Attestation目录
2. 运行./RAClient 机器2的ip 机器1的ip （如，./RAClient 192.168.217.133 192.168.217.132）
