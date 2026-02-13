# IPMIView (java) App wrapper for MacOS

## Quick Start

```bash
cd ~
git clone https://github.com/TheCase/IPMIView.app
cd IPMIView.app
bash script.sh
```

You should now have an application icon in the current directory. You can copy it into `/Applications/` or into `~/Applications/` ror run it from the current location if you'd like.

For some versions of macOS, you may also need to add a security exception for `java`; see [Using the KVM Console](#using-the-kvm-console) for details.

## Details

Running the commands in the [Quick Start](#quick-start) section above will automatically:

1) Clone this repository
2) Run the containing `script.sh`
    1) Download the needed files from SuperMicro's website
    2) Verify the downloaded files
    3) Extract and make the needed modifications to run on macOS
    4) Install the application to `~/Applications/IPMIView.app`

### Download Information

The script in this repo downloads files from SuperMicro's website located at: https://www.supermicro.com/wdl/utility/IPMIView/Linux/

### Intel x86-64 versus Apple Silicon

While the IPMIView application from SuperMicro is mostly written in Java, there are some native binaries required for the KVM console and possibly other parts of it. Oddly enough, the Linux bundle contains binaries for MacOS, but they are x86-64 *only*.

If you are using MacOS on Apple Silicon (aka arm64 aka aarch64), it's still possible to run these x86-64-only binaries as long as you have:

1. Rosetta 2
2. Java Development Kit for MacOS x86-64

When building on MacOS aarch64, `script.sh` will prompt you to install Rosetta 2 and a usable JDK. You can simply set `$JAVA_HOME` to point to that JDK before running `script.sh`.

### Using the KVM Console

You need to add an `Input Monitoring` exception for `java` in the `Security & Privacy -> Privacy` Tab in `System Preferences`:

- Open `System Preferences`
- Click on `Security & Privacy`
- Click the `Privacy` tab
- Scroll down to `Input Monitoring` (you may need to click the lock in the lower left and enter your password to add a new item)
- Click the plus `+` symbol
- In the top of the new window, select `Macintosh HD` in the pulldown `Library -> Java -> JavaVirtualMachines -> jdk<version>.jdk -> bin -> Contents -> Home -> bin`
- Double click on `java`
- Make sure the box next to `java` is now checked and close the window
 - (You may also have to do this for the Java embedded in the IPMIView.app bundle)

When you attempt to launch the console, you may be presented with a message that says the developer is not verified. DO NOT click "Move to Trash" - this will delete the files necessary to run the graphical console. Once you get this message:

- Open `System Preferences` -> `Security & Privacy` -> `General` Tab and click `Allow Anyway` next to the message about the jnlilib that was blocked.
- At this point you can try the `Launch KVM Console` button. You should be presented with another dialog about developer verification. Click the `Open` button.
- This will trigger another denial window for the sharedLibs jnlilib. Repeat the approval process for this next jnlilib in the `Security Preference` Pane.
- After performing these two approvals, the console should open.

## Troubleshooting

If you have Java issues loading the app, please verify that you can run the app from the command line (and outside the jursdiction of this supplied wrapper).

```bash
cd ~/Applications/IPMIView.app/Contents/Resources/IPMIView/
java -jar IPMIView20.jar
```

If you have issues with IMPIView loading correctly with this method, please contact SuperMicro support. The problem is related to the app and your computer setup, not the wrapper.

Remember, if you are using MacOS aarch64, you will need to use an x86-64 version of the JDK/JRE in order to run the KVM Console.
