<h3>DISCLAIMER: This guide asumes that you can understand English well. I will not be responsible for any damage done to your device.​</h3><h4><b>This guide is based on Magisk, but it can be adapted for other systemless boot-image-based root solutions like APatch, Kitsune mask, etc.</b>​</h4><br>
So there are a lot of myths, rumors, fake information and outdated steps regarding rooting, which are causing trouble for newbies who want to root their devices. I've decided to end all of this and create this guide which shows the correct method of rooting.<br>
<br>
<b>Before following the steps, make sure you have the latest version of </b><a href="https://developer.android.com/tools/releases/platform-tools" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener"><b>platform-tools</b></a><b> (i.e ADB and fastboot) on your PC. It is available for GNU/Linux, Windows and MacOS.</b><br>
(If you don't have a PC, you can use the <a href="https://drive.google.com/file/d/15XT8FvcPKUH-fOsJTTUauEdC87fJevyp/view?usp=drive_link" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">Bugjaeger</a> <b>[1] </b>app to use platform-tools from another phone through OTG connection.)<br>
<h2>Android 6.0+​</h2><b>Here are the steps:</b>
<br><br>
1. Unlock the bootloader (Most probably will require PC access)<br>
<br>
<details>
  <summary>Spoiler: How to unlock the bootloader</summary>

<div class="bbCodeSpoiler-content">
<br>
<a href="https://github.com/melontini/bootloader-unlock-wall-of-shame" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">This page</a> shows the method of unlocking the bootloader on major brands and SoCs.<br>

If none of these work for you, try the traditional method:<br>
1. Go to Settings &gt; About device (or something similar) and tap "Build number" several times rapidly until you get the "You are now a developer!" toast.<br>
2. Enable OEM unlocking in Developer Options, and reboot to fastboot/bootloader mode.<br>
3. Connect the device to your PC (or the phone with Bugjaeger) and run:<br>


<div class="bbCodeBlock bbCodeBlock--screenLimited bbCodeBlock--code">
	<div class="bbCodeBlock-title">
		Code:
	</div>
	<div class="bbCodeBlock-content" dir="ltr">
		<pre class="bbCodeCode" dir="ltr" data-xf-init="code-block" data-lang=""><code>fastboot flashing unlock
OR
fastboot oem unlock</code></pre>
	</div>
</div>(If one fails, try the other)<br>
4- Confirm the unlock by pressing the Volume UP button.<br>
5. All your data will be erased and the device will be unlocked.</div>
		</div>
	</div>
</div><hr>
</details>
2. Get the boot.img (A-only devices) or boot_&lt;slot&gt;.img<b> [2]</b> (A/B devices). There are many ways to get them, but I'd recommend you get them by downloading and extracting the exact version of the ROM you're currently on. <b>Do NOT download loose images from the internet, not even from XDA!<br>
</b>
<br>
<details>
	<summary>Spoiler: Other ways to obtain the boot image</summary>
			<div class="bbCodeBlock-content"><h2>1. For A/B devices (universal):​</h2>Temporarily install a pre-rooted ("bvS") GSI (Generic System Image) which corresponds to your current Android version (For example, <a href="https://sourceforge.net/projects/andyyan-gsi/files/lineage-21-td/lineage-21.0-20250322-UNOFFICIAL-arm64_bvS.img.gz/download" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">this</a>) using the <a href="https://f-droid.org/en/packages/vegabobo.dsusideloader/" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">DSU Sideloader</a> app (DO NOT use fastbootd or anything else, as we only want the GSI temporarily). Remember to unzip the .gz/.xz file first to get the GSI's .img file. After rebooting into the GSI, enable USB Debugging and type any one of these ADB commands:<br>

	
	


<div class="bbCodeBlock bbCodeBlock--screenLimited bbCodeBlock--code">
	<div class="bbCodeBlock-title">
		Code:
	</div>
	<div class="bbCodeBlock-content" dir="ltr">
		<pre class="bbCodeCode" dir="ltr" data-xf-init="code-block" data-lang=""><code>adb shell su -c dd if=/dev/block/bootdevice/by-name/boot_&lt;slot&gt; of=/sdcard/boot_&lt;slot&gt;.img
OR
adb shell su -c dd if=/dev/block/bootdevice/by-name/init_boot_&lt;slot&gt; of=/sdcard/init_boot_&lt;slot&gt;.img</code></pre>
	</div>
</div>(Grant root if asked)<br>
Now pull the boot image to your PC using ADB:<br>

	
	


<div class="bbCodeBlock bbCodeBlock--screenLimited bbCodeBlock--code">
	<div class="bbCodeBlock-title">
		Code:
	</div>
	<div class="bbCodeBlock-content" dir="ltr">
		<pre class="bbCodeCode" dir="ltr" data-xf-init="code-block" data-lang=""><code>adb pull /sdcard/boot_&lt;slot&gt;.img
OR
adb pull /sdcard/init_boot_&lt;slot&gt;.img</code></pre>
	</div>
</div>The boot image will be saved in the mtkclient folder.<br>
<h2>2. Most Mediatek devices:​</h2>You can use <a href="https://github.com/bkerler/mtkclient" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">mtkclient</a> to pull the boot image from Mediatek devices. It can be used for many other things, for example to unlock the bootloader (as you may already know from the link I gave for bootloader unlock methods). The installation instructions are clearly given on the GitHub page, so I won't provide additional steps. Once you have installed mtkclient, you can use these commands:<br>

	
	


<div class="bbCodeBlock bbCodeBlock--screenLimited bbCodeBlock--code">
	<div class="bbCodeBlock-title">
		Code:
	</div>
	<div class="bbCodeBlock-content" dir="ltr">
		<pre class="bbCodeCode" dir="ltr" data-xf-init="code-block" data-lang=""><code>For A-only devices:
mtk r boot boot.img
For A/B devices:
mtk r boot_&lt;slot&gt; boot_&lt;slot&gt;.img
OR
mtk r init_boot_&lt;slot&gt; init_boot_&lt;slot&gt;.img</code></pre>
	</div>
</div><h2>3. Tecno/Infinix devices with Unisoc (Spreadtrum) CPU:​</h2>(Please correct me if I'm wrong about if it's Unisoc-specific or not or if it works for Infinix or not, I've tried this only on a Tecno KL4 with Unisoc processor)<br>
You can use the following fastboot commands to get the boot image:<br>

	
	


<div class="bbCodeBlock bbCodeBlock--screenLimited bbCodeBlock--code">
	<div class="bbCodeBlock-title">
		Code:
	</div>
	<div class="bbCodeBlock-content" dir="ltr">
		<pre class="bbCodeCode" dir="ltr" data-xf-init="code-block" data-lang=""><code>A-only devices:
fastboot oem pull boot
A/B devices:
fastboot oem pull boot_&lt;slot&gt;
OR
fastboot oem pull init_boot_&lt;slot&gt;</code></pre>
	</div>
</div>The boot image file will be saved as a file called <b>data.out</b>. Change the extension to .img and rename it accordingly. Funny thing is that it works even with the bootloader locked and OEM unlocking disabled, but since Step 1 was to unlock your bootloader, I asume your bootloader is unlocked at this point.</div>
		</div>
	</div>
</div><hr>
</details>
3. Download the latest <a href="https://github.com/topjohnwu/Magisk/releases" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">Magisk app</a><br>
4- Tap the first "Install" button in the app<br>
5. Tap "Select and patch a file", and navigate to the location of the boot image.<br>
<b>NOTE: </b>Some devices, like some Android 9 ones, have dm-verity and force encryption. In this case, tapping the "Install" button will show you two checkboxes. Uncheck all of them, and click Next to reveal the "Select and patch a file" option.<br>
6. Select the file. Magisk will patch the boot image and store it as /storage/emulated/0/Download/magisk_patched_xxx.img<br>
7. Transfer the patched image to your PC (or the phone with Bugjaeger) and flash it through fastboot:<br>

	
	


<div class="bbCodeBlock bbCodeBlock--screenLimited bbCodeBlock--code">
	<div class="bbCodeBlock-title">
		Code:
	</div>
	<div class="bbCodeBlock-content" dir="ltr">
		<pre class="bbCodeCode" dir="ltr" data-xf-init="code-block" data-lang=""><code>A-only:
fastboot flash boot magisk_patched_xxx.img
A/B:
fastboot flash init_boot_&lt;slot&gt; magisk_patched_xxx.img
OR
fastboot flash boot_&lt;slot&gt; magisk_patched_xxx.img</code></pre>
	</div><br>
</div><b><u>Common misconception:</u></b> some people have suggested that you need to flash a patched vbmeta image for magisk to work properly. This is <b>NOT</b> needed, Magisk will handle everything automatically.<br>
<br>
8. Reboot into Android and open the Magisk app again.<br>
9. You will see the "Requires Additional Setup" prompt. Tap OK and let the device reboot.<br>
10. After the device has finished booting, you should have proper root access. Congratulations!<br>
<br>
<h2>Android 5.1 or below​</h2>Magisk has officially dropped support for Lollipop and below. You can follow the above steps, but instead of using the latest magisk, you should use Magisk 24.3 or below. You can also use SuperSU instead.<br>
<br>
<h2>Samsung devices​</h2>Samsung devices don't have fastboot mode. They've made their own thing called Download mode. To root, you have to get your whole stock ROM in .tar format (not just the boot image), patch it through Magisk, and flash this patched firmware through Odin in Download mode.<br>
<b>NOTE: </b>I wouldn't recommend rooting Samsung devices, as their devices have a "fuse" which gets triggered when you unlock your bootloader and try to flash a custom image (i.e it trips Knox). This results in the permanent loss of all Knox features, and even locking the bootloader wouldn't help unless you get the whole motherboard replaced. Although you can use <a href="https://github.com/salvogiangri/KnoxPatch" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">KnoxPatch</a> (which is a <a href="https://github.com/mywalkb/LSPosed_mod/releases" target="_blank" class="link link--external" rel="ugc nofollow ugc noopener">LSPosed</a> module) to get back most of the features. <b>Root at your own discretion!</b><br>
<br>
<h2><b>KernelSU</b>​</h2><b>KernelSU </b>(and its derivatives such as SukiSU, KSU next, etc) is a modern, kernel-based root solution, which serves as an alternative to Magisk, Kitsune Mask, Apatch, etc. To install it, there are two methods, depending on your device:<br>
<h3>GKI devices​</h3>These include devices whose kernel version is higher than or equal to 5.10.<br>
For these devices, the process is the same as rooting with Magisk. Just download the KernelSU app, patch your boot image and then flash the patched image. You can get the app here:<br>

	

					
<a href="https://github.com/tiann/KernelSU/releases" class="link link--external fauxBlockLink-blockLink" target="_blank" rel="ugc nofollow ugc noopener" data-proxy-href="">
						Releases - tiann/KernelSU
					</a>

						
</span>
				</div>
			</div>
		</div>
	</div>
<h3>Non-GKI devices​</h3>These are older devices which have lower (non-GKI) kernel versions, for example 4.19.<br>
To use KernelSU on these devices, you need to find a supported custom kernel, or compile one on your own. Then, you need to flash the custom kernel through TWRP or some other custom recovery.<br>
<br>
After you have completed the above process, just install the KernelSU app, which you can get from the link above. Note that for <b>non-GKI</b> devices, you will need to install <b>version v1.0.1 or older</b> instead of the latest version, as later versions of the app won't let you install modules. To grant root to any app, you need to go to the Superuser tab and <b>manually</b> enable superuser for that app.<br>
Beware that KernelSU is <b>extremely</b> buggy, and (for non-GKI devices) once you have installed/compiled a kernel with KernelSU, there is <b>no way </b>to remove it unless you change your kernel (or modify the kernel source to remove it).<br>
<br>
<b><u>NOTE: </u>Compiling a custom kernel is out of the scope of this guide. I can only wish you good luck if you are going this way.</b><br>
<br>
<h2>Troubleshooting:​</h2>
<h3>1. Bootloop​</h3>
If you are getting bootloop, it either means that you used a wrong boot image or you need a different version of Magisk.
Ensure that you used the boot image from the current version of your installed ROM, and if it still doesn't work, try different versions of Magisk.
<br>
<h3>2. Weird issues like screen glitch, WiFi/BT/GPS/Camera/RIL not working, etc.​</h3>
It means you used a wrong boot image or downloaded a pre-patched one from the internet. Get the boot image from the current version of your ROM and patch it.
<br>
<h3>3. I flashed the patched boot image, but Magisk app does not detect that it is installed?</h3>​
Try different versions of Magisk, or use an alternative like APatch or Kitsune Mask.
<br>
<h3>4- Device not detected by adb/fastboot​</h3>
If you have Windows, ensure you installed the correct drivers. Otherwise, try different USB cables and ports.
On GNU/Linux or macOS, check if your device is detected by <code>sudo lsusb</code>. If it isn't, it usually indicates a damaged USB port either on the device or the computer, or a damaged cable.
<br>
<h3>5. Banking apps are detecting root!​</h3>
There is a huge cat-and-mouse game going on regarding this. There are tons of modules and methods to hide root from apps, but I won't go into those as I don't support their philosophy. If you root your device, you should be proud of it, and not "hide" it. I suggest you to remove the "banking" app and use cash.
<br>
<h2>References​</h2><b>[1]:</b> Use only the Bugjaeger app linked here. The "official" one on play store contains spyware and malicious, intrusive ads. It is actually a trojan horse virus. The developer is also very toxic, as you can see from his replies in play store reviews.<br>
<b>[2]:</b> Use <code class="bbCodeInline">fastboot getvar current-slot</code> in fastboot to get the slot ("a" or "b"). You can also use the newly introduced <b>init_boot_&lt;slot&gt;.img</b> present in A/B devices instead of the regular boot image (boot_&lt;slot&gt;.img). <b> NOTE</b>: If you get the boot.img/init_boot.img from the stock ROM, the &lt;slot&gt; ("_a" or "_b") might not be present in the file name.</div>
