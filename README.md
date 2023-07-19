# some-additional-support-for-linux

![visitors](https://visitor-badge.laobi.icu/badge?page_id=SrijanBhattacharyya/some-additional-support-for-linux)

## TOC
* [Multi-boot Setup with GRUB](multi-boot-setup-with-grub)

## Multi-boot Setup with GRUB
Instructions for setting up a multi-boot configuration using GRUB, including Linux Based OSs and Windows.

### Prerequisites

* Atleast one linux based OS
* Windows installed (optional)

### Installation Steps
1. Install GRUB:
   * <details>
      <summary>For OSs with `pacman` as packagemanager:</summary>

     ```
     sudo pacman -S grub
     ```
     </details>
   * <details>
      <summary>For OSs with `apt` as packagemanager:</summary>

     ```
     sudo apt install grub
     ```
     </details>
   > **NOTE:**
   <br> If GRUB is already installed in your OS, you can skip this step.

3. Configure GRUB for Linux based OS:
   * Edit the GRUB configuration file:
     ```
     sudo nano /etc/default/grub
     ```
   * Customize the GRUB settings as desired.
   * Save the changes and exit the text editor.
   * Update GRUB:
     ```
     sudo grub-mkconfig -o /boot/grub/grub.cfg
     ```

4. Add an OS to GRUB:
   * <details>
      <summary>Linux based OS</summary>

     * Open the custom configuration file for editing:
       ```
       sudo nano /etc/grub.d/40_custom
       ```
     * Add the following lines to the file:
       ```
       menuentry <NAME_OF_OS_WHICH_YOU_WANT_TO_SHOW> {
         insmod part_gpt
         insmod fat
         insmod search_fs_uuid
         search --fs-uuid --no-floppy --set=root <ARCH_ESP_UUID>
         chainloader <PATH_TO_OS_EFI.efi>
       }
       ```
       > *NOTE:*
       <br> Replace `<NAME_OF_OS_WHICH_YOU_WANT_TO_SHOW>` with your OS name [like: "Arch Linux"].
       <br> Replace `<ARCH_ESP_UUID>` with UUID of the EFI System Partition (ESP) where bootloader of the OS is located. *You can find the UUID by running the command `sudo blkid /dev/sdXY`, where `/dev/sdXY` is the partition device for the ESP, for example: `/dev/sda1`.*
       <br> Replace `<PATH_TO_OS_EFI.efi>` with the path to the EFI of the os. Usually present at `/EFI/arch/vmlinuz.efi` or `/boot/efi/EFI/<OS_NAME>/grubx64.efi`. Here `<OS_NAME>` is the name of your OS.
     * Save the changes and exit the text editor.
     * Update GRUB:
       ```
       sudo grub-mkconfig -o /boot/grub/grub.cfg
       ```
     </details>

   * <details>
      <summary>Windows</summary>

     * Open the custom configuration file for editing:
       ```
       sudo nano /etc/grub.d/40_custom
       ```
     * Add the following lines to the file:
       ```
       menuentry "Windows" {
           insmod ntfs
           set root=(hd0,1)
           chainloader +1
       }
       ```
       > **NOTE:**
       <br> Customize the `set root` line if necessary to reflect the correct partition for Windows.
     * Save the changes and exit the text editor.
     * Update GRUB:
       ```
       sudo grub-mkconfig -o /boot/grub/grub.cfg
       ```
     </details>

5. Reboot your system and verify the GRUB menu displays options for the newly added OSs.

### Troubleshooting
If you encounter any issues during the installation or configuration process, try the following steps:

* Double-check the UUIDs and partition numbers in the GRUB configuration files to ensure accuracy.
* Ensure that the bootloader paths and filenames match the actual file locations on your system.
* If you're having difficulty locating the EFI partition UUID, use the `sudo blkid /dev/sdXY` command, where `/dev/sdXY` corresponds to the EFI partition device.

## Contributing

Contributions to this project are welcome! If you have any suggestions or improvements, please feel free to submit a pull request.

## License

This project is licensed under the [GNU GENERAL PUBLIC LICENSE](LICENSE).
