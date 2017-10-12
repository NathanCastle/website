# Redefining the Windows Desktop Experience... with Linux

> Originally published on 02/28/2017

To most PC users, there is no distinction between the desktop environment and the OS itself. This simplicity, combined with the generally reliable Explorer desktop experience, goes a long way to explaining the success of the Windows desktop. Yet, some users yearn for more; when I talk to Linux desktop users, the #1 reason given is that the desktop is more configurable.

While reading about the Windows logon process, I was inspired; why not use the WinLogon extensibility interface (extensions being called "Credential Providers") to flip the script and launch a Linux desktop instead of Explorer. Most Linux distros ship with a logon UI that allows selection of a preferred Desktop Environment (DE), so of course I wanted to enable that scenario as well.
<blockquote>Before I get into this, quick clarification about the title: Linux Desktop Environments have absolutely nothing to do with Linux, the kernel; they just happen to run on Linux-based systems most of the time. When I refer to Linux DEs, know that these are desktops that may or may not run on Linux; in this case they're running on Windows via the WSL.</blockquote>
To accomplish this task, I realized I'd need a few components:
<ul>
	<li>Something to let me capture the user's preference at the logon screen</li>
	<li>Some way to store the user's preference and read it back after the login</li>
	<li>Something to pre-empt Explorer and either run the traditional desktop or the chosen DE post-login</li>
	<li>Some way to configure all of this (convenience is key)</li>
</ul>
Although Credential Providers are intended to enable new authentication scenarios (pin-based login and Windows Hello take advantage of this feature), they also serve as a convenient way to run code that interacts with the Logon UI. The Credential Provider does the following things:
<ul>
	<li>Check the registry to find the chosen DEs</li>
	<li>Display a combobox & tile on the logon screen</li>
	<li>Update the registry to reflect the chosen DE</li>
	<li>Take you password and let you login (just like the default experience)</li>
</ul>
Just changing some things in the registry isn't enough. Now I needed to get code running immediately after logon to actually launch the DE of choice. WinLogon is configured with several settings in the registry under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\WinLogon. The ones to note are:
<ul>
	<li><strong>Shell</strong>: defines the shell executable. Post-logon this ostensibly selects which shell to run. The semantics are slightly different in reality: Explorer.exe looks at Shell to decide whether or not it will present the start menu and taskbar. If Shell is set to something different (like cmd.exe), the taskbar and start menu won't appear upon starting Explorer.exe. Note that setting shell after the fact doesn't automatically start the shell experience; I take advantage of this later.</li>
	<li><strong>Userinit</strong>: defines the program that runs to get the session started. Userinit.exe is there by default, but this is a comma-separated list. Userinit.exe does nice things, so I keep it around and add my program to the list. Note that attempting to change this outright (removing userinit.exe from the list) results in Windows resetting it. Joke's on you if you attempt to fool WInLogon. Userinit.exe also gives you an explorer shell for "free"; you have no way of controlling this from what I gather.</li>
</ul>
<blockquote>Broken Abstraction Alert: UWP apps will not respond to changes in their frame size if Windows Explorer is not running <strong>and</strong> set as the Shell under WinLogon.  The reason for this is unclear, but likely has something to do with why Edge tabs and the like are so buggy. Look forward to a future blog post.</blockquote>
The next part of my project is a small Windows program called ConfigurableShell.exe. ConfigurableShell takes the following action (it is called after Userinit.exe):
<ul>
	<li>Sets itself as the Shell (to prevent shenanigans)</li>
	<li>Loads up the options for the selected DE</li>
	<li>Starts VcXsrv (a more robust fork of Xming) as the Xserver of choice, configured to run fullscreen</li>
	<li>Resets explorer.exe as the shell (to unbreak various Windows things)</li>
	<li>Calls bash.exe or cmd.exe to load the DE of choice</li>
</ul>
With these pieces, it is now possible to boot Windows directly into a Linux DE. Note the following components need to be installed:
<ul>
	<li><strong>RegProvider.dll</strong>: helper library I created for working with the registry. Goes to C:\Windows\System32</li>
	<li><strong>BootShellCredentialProvider.dll</strong>: Credential Provider dll (used by WinLogon). Goes to C:\Windows\System32
<ul>
	<li>Note, also needs to be registered in several places. This is handled by the Configurator.</li>
</ul>
</li>
	<li><strong>ConfigurableShell.exe</strong>: Called during the logon process to start selected shell. Goes to C:\Windows\System32</li>
	<li><strong>BSCP Configurator</strong>: .NET app configures the credential provider. Installed into Program Files with shortcut in Start Menu</li>
</ul>
Because there is so much that needs to be done for the installation, I created an installer to make things easy. Unfortunately, actually getting the Linux desktops running isn't easy for a variety of reasons. To make things easy, I've put together the steps I took to get things running in a <a href="https://github.com/NathanCastle/BootShellCredentialProvider/blob/master/BSCP/Configurator/wsl_setup.sh" target="_blank" rel="noopener">.sh script</a>. This is approximately what I did, starting from scratch, so hopefully its useful.

Check it out on <a href="https://github.com/NathanCastle/BootShellCredentialProvider" target="_blank" rel="noopener">GitHub</a>.
