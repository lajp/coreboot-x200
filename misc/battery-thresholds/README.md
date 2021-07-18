# battery-thresholds

"A systemd-service to apply battery thresholds on a corebooted X200"

I have no idea how it works, but it uses the ectool-binary located in coreboot/ectool.

It applies the following thresholds [ST=50, SP=80]

I do realize that there is most likely a way more elegant way to do this, but I don't really care
since this approach works for me without issues.
