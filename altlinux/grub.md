set pages=1
ls
set root=(hd0,gpt3)
ls /@/boot
linux /@/boot/vmlinuz-6.12
initrd /@/boot/vmlinuz-6.12.img
boot
