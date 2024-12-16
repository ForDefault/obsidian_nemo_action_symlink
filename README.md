# obsidian_nemo_action_symlink
Obsidian Nemo Action: syslink to folder obsidian_notes (need to install Symlink Creator
[Symlink Creator](https://github.com/pteridin/obsidian_symlink_plugin))

# I am using this to aggregate docker in the path 
$obsidian_target_folder so change the 

```
username="$(whoami)"
obsidian_target_folder="/home/$username/obsidian_notes/docker"
icon_path="$(cp /home/$username/.cache/discover/icons/md.obsidian.Obsidian.png /home/$username/.local/share/icons/ && echo /home/$username/.local/share/icons/md.obsidian.Obsidian.png)"

# Create the Nemo action and script in the same folder
mkdir -p /home/$username/.local/share/nemo/actions

# Create the Nemo action file
cat <<EOF > /home/$username/.local/share/nemo/actions/link_to_obsidian.nemo_action
[Nemo Action]
Name=Link to Obsidian Notes
Comment=Create a symlink to this folder in Obsidian Notes
Exec=/home/$username/.local/share/nemo/actions/link_to_obsidian.sh %F
Icon-Name=$icon_path
Selection=Any
Extensions=dir;
EOF

# Create the corresponding script
cat <<EOF > /home/$username/.local/share/nemo/actions/link_to_obsidian.sh
#!/bin/bash

# Get the target path from Nemo action
TARGET_PATH="\$1"

# Check if the target path is a directory
if [ -d "\$TARGET_PATH" ]; then
    # Create a symbolic link
    ln -s "\$TARGET_PATH" "$obsidian_target_folder"
    notify-send "Symbolic Link Created" "Linked \$TARGET_PATH to $obsidian_target_folder"
else
    notify-send "Error" "\$TARGET_PATH is not a valid directory."
    exit 1
fi
EOF

# Make the script executable
chmod +x /home/$username/.local/share/nemo/actions/link_to_obsidian.sh

```
