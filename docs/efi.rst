EFI
===

SecureBoot
----------

If after installing Linux Mint in EFI mode, you are unable to boot due to a ``Secure Boot Violation``, you can try one of the following solutions:

.. figure:: images/secureboot-violation.jpg
    :width: 500px
    :align: center

* Restart the installation:
    * Connect to the Internet before the installation
    * **Do not** select ``Install third-party software for graphics and Wi-Fi hardware, Flash, MP3 and other media``.

* Disable ``SecureBoot`` in the ``BIOS`` settings of your computer.

  See `here <http://www.rodsbooks.com/efi-bootloaders/secureboot.html>`_ for examples of how to disable secure boot on various computers.

EFI boot order
--------------

If after installing Linux Mint in EFI mode, your computer skips the boot menu and boots straight into Windows (or another operating system), you probably have an issue with the boot order.

To modify the boot order:

1. Boot Linux Mint in ``live`` mode (with your USB stick or DVD).

2. Open a terminal.

3. Type ``sudo efibootmgr``.

This command lists the available boot options and the boot order.

.. figure:: images/efibootmgr.png
    :width: 500px
    :align: center

In the screenshot above, there are three boot options:

* ``ubuntu`` at ``0000``
* ``linuxmint`` at ``0001``
* ``Mac OS X`` at ``0081``

The boot order is ``0081``. This indicates that the computer only tries to boot Mac OS and not Linux Mint.

.. important::
    For technical reasons Linux Mint uses ``ubuntu`` as its EFI boot name.


4. To fix the boot order, type ``sudo efibootmgr --bootorder XXXX,YYYY`` (where ``XXXX`` and ``YYYY`` are the operating system boot options you want to boot).

.. figure:: images/efibootmgr-2.png
    :width: 500px
    :align: center

In the screenshot above, ``sudo efibootmgr --bootorder 0000,0081`` instructs the computer to first try to boot Linux Mint (``ubuntu`` being the EFI boot name for Linux Mint), and then Mac OS.

5. Restart the computer.

.. note::
    In the screenshot above ``0000`` is the first boot option so the computer boots on the Linux Mint grub menu. If grub fails (or if it is dismissed with the ``exit`` command), the computer follows the boot order and then tries to boot ``0081``, which corresponds to Mac OS.

Further troubleshooting for PCs
-------------------------------

If your computer boots straight into windows, try the following solutions:

* In the BIOS settings of your computer, place the entry ``ubuntu`` at the top of the boot order
* From windows:

    * Use `EasyUEFI <https://www.easyuefi.com/index-us.html>`_ to adjust the boot order, placing ``ubuntu`` at the top
    * From an administrative command prompt, use `bcdedit <https://itsfoss.com/no-grub-windows-linux>`_ to adjust the boot priority:

        ``bcdedit /set {bootmgr} path \EFI\ubuntu\shimx64.efi``

Some computer brands have particular issues requiring specific workarounds:

* Acer computers: `select shim as trusted for executing <https://itsfoss.com/no-bootable-device-found-ubuntu>`_
* HP computers: `make fake windows boot files <https://forums.linuxmint.com/viewtopic.php?f=42&t=243338&p=1300722>`_
