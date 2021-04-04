# TTS voice generator script for Roborock S5 Max

This is a script that generates audio files from texts using google's text-to-speech api. Similar to https://github.com/dgiese/dustcloud/tree/master/devices/xiaomi.vacuum/audio_generator.

The script reads the texts used to generate the sound files from a toml file. See the `texts_en.toml` file for default english texts.

## How to use

Install dependencies with `pip3 install -r requirements.txt` or

    sudo apt install python3-certifi python3-chardet python3-idna python3-requests python3-six python3-toml python3-urllib3 python3-click python3-gtts
    pip3 install --user ffmpeg-python
    pip3 install --upgrade --user gtts

and run the script with `python3 src/voicegen -c texts_en.toml -o output_dir`
 

## Additional notes

Voice files for S5 Max can be found in 
̉̉`/mnt/resources/audio_default` and `/mnt/resources/audio_custom`.

`/mnt/resources/audio_default` is mounted to `/dev/nandi` and 
`/mnt/resources/audio_custom` is mounted to `/dev/nandj`.
If you have changed the robot's voice to something other than the default voice, the audio_custom is used.


You can use the following commands to customize the voice **(USE AT YOUR OWN RISK)**:

S5 Max:
* `umount /dev/nandj`
* `dd if=/dev/nandj of=/tmp/sounds.sqfs`
* `mount /dev/nandj /mnt/resources/audio_custom`

PC:
* `scp root@robot_ip:/tmp/sounds.sqfs sounds.sqfs`
* `unsquashfs sounds.sqfs`
* `python3 src/voicegen -c texts_en.toml -o squashfs-root/sounds`
* `rm -f new_sounds.sqfs` if we don't do this we will store the old data too.
* `(cd squashfs-root/tmod && ln -sf ../sounds/tmod_start_upload_log.ogg tmod_start_upload_log.ogg) && (cd squashfs-root/ && ln -sf sounds/start_greeting.ogg start_greeting.ogg)`
* `mksquashfs squashfs-root new_sounds.sqfs`
* `scp new_sounds.sqfs root@robot_ip:/tmp/sounds.sqfs`

S5 Max:
* `umount /dev/nandj`
* `dd if=/tmp/sounds.sqfs of=/dev/nandj`
* `mount /dev/nandj /mnt/resources/audio_custom`
