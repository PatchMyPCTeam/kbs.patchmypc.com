# ConfigApi is not responding. An unhandled exception occurred

In this article we troubleshoot why this error might occur when launching the Patch My PC Publisher UI and how to resolve it.

### Determine if You are Affected <a href="#h-determine-if-you-are-affected" id="h-determine-if-you-are-affected"></a>

When you launch the Publisher, you might receive the following error.

The PatchMyPCService is running, but the ConfigApi is not responding. Cloud features will not be available. Please see PatchMyPC.log for more information

<figure><img src="https://patchmypc.com/app/uploads/2025/04/ConfigApi_Error.png" alt=""><figcaption></figcaption></figure>

When you click OK, you get a further warning.

An unhandled exception occurred: :9001]

<figure><img src="https://patchmypc.com/app/uploads/2025/04/ConfigApi_Error_UnhandledException.png" alt=""><figcaption></figcaption></figure>

Both of these errors can be found in the %ProgramFiles%Patch My PCPatch My PC Publishing ServicePatchMyPC.log

<figure><img src="https://patchmypc.com/app/uploads/2025/04/ConfigApi_Error_Log-1.png" alt=""><figcaption></figcaption></figure>

Run the following command to verify you are missing a configuration setting on the host which means the Publisher cannot bind to the ConfigApi on the loopback adapter.

**netsh http show iplisten**

<figure><img src="https://patchmypc.com/app/uploads/2025/04/ConfigApi_Error_netsh.png" alt=""><figcaption></figcaption></figure>

If 127.0.0.1 is not present in the list, this will cause the error listed in this article.

### Workaround <a href="#h-workaround" id="h-workaround"></a>

The ConfigApi is bound to the loopback adapter address 127.0.0.1 or the DNS name LOCALHOST. A legacy misconfiguration can result in the IP Listener configuration for 127.0.0.1 being removed from the host.

Run the following command to add 127.0.0.1 back as an IP Listener.

**netsh http add iplisten 127.0.0.1**

Verify the addition of 127.0.0.1 was successful by running the following command.

**netsh http show iplisten**

<figure><img src="https://patchmypc.com/app/uploads/2025/04/ConfigApi_Error_netshFix.png" alt=""><figcaption></figcaption></figure>
