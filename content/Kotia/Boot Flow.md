I wanted to understand the complete boot flow from Hardware to software 
i.e. When you press the power button to your first user space process,
A lot of content is there, so this is me being selfish trying to assume that people want ot hear about it more and in depth, and I want to understand it too.

Flow as iknow
Power button -> PMIC -> CPU reset vector -> POST -> Bootloader -> GRUB/UEFI -> Kernel -> user space process

Even as i start we are at an issue, i can't seem to find some good sources on understanding the hardware -> bootloader layer, so rather than understanding from Left to Right, let's go from 
GRUB -> Kernel -> user, and once that's explore we could maybe circle back, this is a better approach so that i could actually start the work rather than procrastinating over unfinished information

So after bootloader what happens