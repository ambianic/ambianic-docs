# Ambianic Box

[![Join the Slack chat room](https://img.shields.io/badge/Slack-Join%20the%20chat%20room-blue)](https://join.slack.com/t/ambianicai/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ)

Ambianic Box is the recommended enclosure for DIY (do-it-yourself) installations of Ambianic Edge. It is (yet another) Open Source Smart Camera ecnlosure for Raspberry Pi 4B with a Raspberry Pi Camera.

# Why does Ambianic Box exist?

Proprietary camera manufacturers don't make it easy to access their products via open APIs. RTSP is available in some models, but hard to figure out. ONVIF is partially supported and usually hidden behind several proprietary enablement steps. Turned off by defauly more often than not. It often requires installing custom, unsupported firmware in order to open RTSP or RTMP access. And there is no telling if and when that option may be discontinued. It depends on company policy.

Most camera manufacturers want you to use their own app for access to their cameras and integration into their own home automation platforms. They are protecting current and future revenue. Trying to please multiple masters with different demands: end users who want a great product and investors who want great ROI. That creates a trend to move towards centralized cloud subscription and data collection services which have been proven prone to hacks and massive user data leaks. It is hard to build trust with users without verifiable code transparency proving protection of user data privacy. 

So here we go. Let's build something that works for Open Source developers and DIY folks.

<img src="https://github.com/ambianic/ambianic-box/raw/main/ambianic_box_rendering.png" height="400"/>

You can 3D Print an Ambianic Box at home ([here is the source](https://github.com/ambianic/ambianic-box)) or order a 3D print online through a service such as [craftcloud3d.com](https://craftcloud3d.com/configuration/c7faee7e-ec06-41ed-822c-4092bdf1d28d) or [makexyz.com](https://www.makexyz.com/).

Alternatively, if you don't want to 3D print an enclosure, you can order a kit such as [this ony by Labists](https://labists.com/collections/all/products/labists-raspberry-pi-4g-ram-32gb-card) which includes a usable enclosure box and other components required to build an Ambianic Edge device.

# Components

To assemble a fully functional Ambianic Box, we recommend the following readily available off the shelf components, but you can use any viable alternatives available to you:

| Component  | Retail Price | Link(s) to Online Stores |
| ------------- | ------------- | ------------- |
| Raspberry Pi 4 B 2GB | $35  | [RaspberryPi.org](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/?resellerType=home), [amazon](https://www.amazon.com/Raspberry-Model-2019-Quad-Bluetooth/dp/B07TD42S27/ref=sxts_sxwds-bia-wc-drs1_0?cv_ct_cx=raspberry+pi+4+2gb&dchild=1&keywords=raspberry+pi+4+2gb&pd_rd_i=B07TD42S27&pd_rd_r=873aac72-20e5-46c0-99ce-9e47ea60881e&pd_rd_w=gOwz9&pd_rd_wg=lG0sq&pf_rd_p=c33e4373-edb9-47f9-a7e6-5d3d6a7a4ad0&pf_rd_r=KRGVVKRVVZTQ5FA0Q55J&psc=1&qid=1605758079&sr=1-1-5e875a02-02b1-4426-9916-8a5c26cd5a14) |
| SD Card 32GB  | $8  | [amazon](https://www.amazon.com/Samsung-MicroSDHC-Adapter-MB-ME32GA-AM/dp/B06XWN9Q99/ref=pd_bxgy_img_3/144-4430494-7772565?_encoding=UTF8&pd_rd_i=B06XWN9Q99&pd_rd_r=a15cd46f-405b-4e68-964d-2abdc4331448&pd_rd_w=iB7pd&pd_rd_wg=GMbRu&pf_rd_p=f325d01c-4658-4593-be83-3e12ca663f0e&pf_rd_r=YFPJN5QH2KR9RWRYBNCG&psc=1&refRID=YFPJN5QH2KR9RWRYBNCG), [Best Buy](https://www.bestbuy.com/site/pny-32gb-microsdhc-uhs-i-memory-card/6327962.p?skuId=6327962) |
| 5V 3A USBC Charger  | $3 | [wish](https://www.wish.com/product/593badce564c7e24a9cf2c9f?hide_login_modal=true&from_ad=goog_shopping&_display_country_code=US&_force_currency_code=USD&pid=googleadwords_int&c=%7BcampaignId%7D&ad_cid=593badce564c7e24a9cf2c9f&ad_cc=US&ad_lang=EN&ad_curr=USD&ad_price=3.60&campaign_id=8701430383&retargeting=true&gclid=Cj0KCQiAqdP9BRDVARIsAGSZ8AmgVu5NhnIR4ciTOxAS0RuKiafialsqoFvQDmOAAsDYKixyqm1DmRQaArF5EALw_wcB&share=web), [ebay](https://www.ebay.com/i/183831264689?var=691473058624&chn=ps&norover=1&mkevt=1&mkrid=711-117182-37290-0&mkcid=2&itemid=691473058624_183831264689&targetid=934793862456&device=c&mktype=pla&googleloc=9028305&campaignid=10455978148&mkgroupid=104612011180&rlsatarget=aud-649939740884:pla-934793862456&abcId=2146002&merchantid=136047574&gclid=Cj0KCQiAqdP9BRDVARIsAGSZ8AnhnfUart_pYDl8wPHKhJl1PHab4U2OtoLLBX1lkuNPQ-9XN6DcfVYaAkMkEALw_wcB), [amazon](https://www.amazon.com/Power-Supply-Adapter-Switch-Raspberry/dp/B07TSDJSQH/ref=sr_1_15?dchild=1&gclid=Cj0KCQiAqdP9BRDVARIsAGSZ8AlYZbWqEj2ea-9odBwcTzQ-Coq0Zzv8hptd5f5q8QX-MqoPuq-NkPIaAvyjEALw_wcB&hvadid=443816411978&hvdev=c&hvlocphy=9028305&hvnetw=g&hvqmt=e&hvrand=14098241450732675696&hvtargid=kwd-920541044600&hydadcr=21947_9709434&keywords=5v%2F3a+usb+charger&qid=1605743063&sr=8-15&tag=googhydr-20) |
| 5MP generic picam  | $5 | [aliexpress](https://www.aliexpress.com/item/32946093276.html?src=google&albch=shopping&acnt=494-037-6276&isdl=y&slnk=&plac=&mtctp=&albbt=Google_7_shopping&aff_platform=google&aff_short_key=UneMJZVf&&albagn=888888&albcp=11491261337&albag=111738685669&trgt=708011819940&crea=en32946093276&netw=u&device=c&albpg=708011819940&albpd=en32946093276&gclid=Cj0KCQiAqdP9BRDVARIsAGSZ8AnSaUcIZU7iDG24QDlKKKp5OlOd3nNHBHSx843SYjAWjgG31dzpadcaAlAqEALw_wcB&gclsrc=aw.ds), [amazon](https://www.amazon.com/gp/product/B07QNSJ32M/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1) |
| 3D Print Material* (130g of PLA) | $3  | [MatterHackers](https://www.matterhackers.com/store/l/175mm-pla-filament-white-1-kg/sk/MEEDKTKU?rcode=GAT9HR&gclid=Cj0KCQiAqdP9BRDVARIsAGSZ8AkzOYaP5s5nvpkNWoCH_eiSJXGnUHVKdi8fQJLOE69DU3W1SD2gkawaAoRIEALw_wcB), [amazon](https://www.amazon.com/HATCHBOX-3D-Filament-Dimensional-Accuracy/dp/B00J0GMMP6/ref=sr_1_3?dchild=1&keywords=PLA+Plastic&qid=1605758618&sr=8-3) |
| **Total**  | **$54** | ____________ |

---
**\*3D Print Note**

If you don't have a 3D printer handy for the box enclosure, you can use an online service, in which case the cost for the enclosue will vary from $20 to $60 depending on time, distance, material and other options.

---


# How to Assemble an Ambianic Box

## Instructional Video

<style >
  .embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
</style>

<div class='embed-container'>
  <iframe src='https://www.youtube.com/embed//Tys3lW9tNAU' frameborder='0' allowfullscreen></iframe>
</div>

## Slides

<style>
  .responsive-google-slides {
    position: relative;
    padding-bottom: 56.25%; /* 16:9 Ratio */
    height: 0;
    overflow: hidden;
  }
  .responsive-google-slides iframe {
    border: 0;
    position: absolute;
    top: 0;
    left: 0;
    width: 100% !important;
    height: 100% !important;
  }
</style>

<div class="responsive-google-slides">
  <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vQcEWyjuR-oSnmyvGxoSlEYzFkivDdDd6EkhaJtHalm5lxHxOrjsywYNkmsz1IxLld3b2ig9Yy-7ytx/embed?start=false&loop=false&delayms=3000#slide=id.p2"></iframe>
</div>



---
**Wall Mount Note**

Ambianic Box can stand up on a flat surface or it can be mounted. For wall mount, there are several options. The box has two  openings on the back side that can be attached to wall mount hooks. Alternatively if you don't feel like drilling into your wall, the box is light enough to mount via two sided mounting tape such as [this one](https://www.amazon.com/gp/product/B07LFRN1K8/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1).

---

# Installing Ambianic Edge

Ambianic Box comes to live once the [Ambianic Edge](https://github.com/ambianic/ambianic-edge) software is installed. The simplest and recommended way to install and run Ambianic Edge is via the pre-built [Ambianic OS Image](https://github.com/ambianic/ambianic-rpi-image).

To get a sense of the full user experience, take a look at the [Ambianic Quick Start Guide](https://docs.ambianic.ai/users/quickstart/).
  
Since all layers of the system are Open Source, you can swap any of them out with your own.

# Other Raspbery Pi Camera Projects and 3D enclosures

* [MotionEyeOS for Raspberry PI Zero](https://github.com/ccrisan/motioneyeos)
* [Raspberry Pi Camera Case made in OpenSCAD](https://youtu.be/mZ9OWpSGRZU)
* [Compact Weatherproof Raspberry Pi Camera Case](https://tinkererblog.wordpress.com/2015/07/28/how-i-designed-a-compact-weatherproof-raspberry-pi-case/)
* [RASPBERRY PI 2 and Camera Case](https://grabcad.com/library/raspberry-pi-2-camera-case-1)
* [Smoothcam Raspberry Pi 4 Camera Case](https://ameridroid.com/products/smoothcam-raspberry-pi-4-camera-case-3d-printed)
* [Raspberry Pi HQ Camera Case](https://learn.adafruit.com/raspberry-pi-hq-camera-case/3d-printing)
* [PiSec MKIII - Compact Edition](https://www.thingiverse.com/thing:2825778)
* [IPi V2i](https://www.thingiverse.com/thing:3727587)
* [Raspberry Pi Zero W Security Camera using NoIR Camera](https://www.thingiverse.com/thing:3816376)
* [RASPBERRY PI AND CAMERA TO SECURITY CAM](https://cults3d.com/en/3d-model/various/raspberry-pi-and-camera-to-security-cam-enclosure-mount-source-files-included)
* [Raspberry Pi Security Camera](https://all3dp.com/9-things-you-need-for-a-3d-printed-raspberry-pi-security-camera/)
* [Raspberry Pi HD surveillance camera](https://tritek.pw/2013/11/01/raspberry-pi-hd-surveillance-camera/)
