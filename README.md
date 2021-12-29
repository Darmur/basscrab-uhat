# BassCrab-uHAT

### uHAT for Raspberry Pi with NXP SGTL5000 Codec, two Rotary Encoders, user LED and socker for OLED display.

![BassCrab-uHAT](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/pictures/pict05.jpg)

### Project description
BassCrab-uHAT is an expansion board for Raspberry Pi Zero, but it will work fine with other Rasbperry Pi versions (2, 3A, 3B, 4) and with ASUS ThinkerBoard too. It's been designed following the mechanical specification for [uHAT](https://github.com/raspberrypi/hats/blob/master/uhat-board-mechanical.pdf) form factor.

Together with [Volumio](https://volumio.com/), it will turn your Raspberry Pi into a tiny but powerful digital audio player/streamer.

BassCrab-uHAT is powered by the following peripherals:

- 1x Audio Codec SGTL5000 from NXP
- 1x Headphone stereo 3.5 mm jack
- 1x Line-out stereo 3.5 mm jack
- 2x Rotary Encoder with push-button
- 1x Ice-Blue LED
- 1x Socket for 0.96" OLED display

The onboard Audio Codec is fully supported in the latest RaspiOS and Volumio images.
To enable it on RaspiOS, please add the following string to the config.txt file:

```
dtoverlay=fe-pi-audio
```

The Rotary Encoders with push-buttons can be used to control the audio playback (play, pause, volume-up, volume-down, previous song, next song, etc.). The onboard LED can be used as an indicator for SD-Card activity or playback status, as an example.

Full documentation is available on this public Github repository, including BOM and SMD positioning files, for automated assembly. Please feel free to download gerber files and documentation, to order PCBs and to copy/improve the design, if you like!

![Front](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/pictures/front.jpg)

![Back](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/pictures/back.jpg)


### Volumio Setup

Download the latest **Volumio 3** image from the [official website](https://volumio.com/en/get-started/), than flash the image on a SD-card, as usual. Please refer to the [Quickstart guide](https://cdn.volumio.com/wp-content/uploads/2021/09/Quick-Start-Guide-Volumio.pdf) for the initial setup of your Volumio-based system.

In the third tab of the initial setup wizard, please **enable** I2S DAC and select **Fe-Pi Audio** from the dropdown list
![Fe-Pi](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/volumio_settings/audio_output.png)

When your system is up and running, please navigate in the **Playback Option** setting page, and enable the resampling as shown in the following picture
![Resampling](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/volumio_settings/audio_resampling.png)

As a last step for the initial configuration of your Volumio system, please **enable SSH** following the [official procedure](https://volumio.github.io/docs/User_Manual/SSH.html).
SSH is required to enable both Headphone and Line-out and to adjust their signal level.

After enabling SSH, please login into your Volumio system and type the following command:

```
alsamixer
```

Right after that, press **F6**, select **Fe-Pi Audio** and press **ENTER**

![Alsamixer](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/volumio_settings/alsamixer_select_card.png)

Use the **left/right** arrows to select the parameter to be changed, **up/down** arrows to adjust the value, **M** key to toggle mute/unmute of the resource. Please make sure to apply the settings as shown in the following picture:

![Alsamixer](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/volumio_settings/alsamixer_settings.png)

Pay attention to the two letters below the level setting: with **MM** the resource is muted, with **OO** the resource is active.


### User LED Setup

BassCrab-uHAT features an Ice-Blue LED. It is connected to GPIO26 of the Raspberry Pi (Pin 37).

##### User LED as a SD-Card Activity indicator

Add the following line to ***userconfig.txt*** (for Volumio) or to ***config.txt*** (for RaspiOS)
```
dtparam=act_led_gpio=26
```

##### User LED as a System or Playback Status indicator

The most common plugins to handle the LED is ***GPIO_Control***.
Unfortunately it is not available yet on the Volumio Plugin Store, but it can be manually installed with SSH, with the sequence of the following commands:
```
sudo apt -y install build-essential
cd ~
wget http://plugins.volumio.org/plugins/volumio/armhf/system_controller/gpio_control/gpio_control.zip
mkdir ./gpio_control
miniunzip gpio_control.zip -d ./gpio_control
cd gpio_control
volumio plugin install
cd /data/plugins/system_controller/gpio_control/
npm install --save onoff@6.0.0
npm install --save sleep@6.2.0
```

When the plugin is installed and enabled, those settings should be applied, for either system status or playback status:

![GPIO-control_system](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/volumio_settings/gpio-control_system.png)

![GPIO-control_system](https://raw.githubusercontent.com/Darmur/basscrab-uhat/main/volumio_settings/gpio-control_playback.png)


