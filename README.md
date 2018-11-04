# Mini Media Player
A minimalistic yet powerful & customizable media player card for [Home Assistant](https://github.com/home-assistant/home-assistant) Lovelace UI.

Inspired by [Custom UI: Mini media player](https://community.home-assistant.io/t/custom-ui-mini-media-player/40135) and [custom-lovelace](https://github.com/ciotlosm/custom-lovelace).

![Preview Image](https://user-images.githubusercontent.com/457678/47517460-9282d600-d888-11e8-9705-cf9ec3698c3c.png)

## Install

### Simple install

- Copy `mini-media-player.js` into your `config/www` folder.
- Add a reference to the `mini-media-player.js` inside your `ui-lovelace.yaml`.

```yaml
resources:
  - url: /local/mini-media-player.js?v=0.8.9
    type: module
```

### Install using git

- Clone this repository into your `config/www` folder using git.

```bash
git clone https://github.com/kalkih/mini-media-player.git
```

- Add a reference to the card in your `ui-lovelace.yaml`.

```yaml
resources:
  - url: /local/mini-media-player/mini-media-player.js?v=0.8.9
    type: module
```

### *(Optional)* Add to custom updater

- Make sure you got the [custom_updater](https://github.com/custom-components/custom_updater) component installed.
- Add a reference under `card_urls` in your `custom_updater` configuration in `configuration.yaml`.

```yaml
custom_updater:
  card_urls:
    - https://raw.githubusercontent.com/kalkih/mini-media-player/master/tracker.json
```

## Updating
 **Important:** If you are updating from a version prior to v0.5, make sure you change `- type: js` to `- type: module` in your reference to the card in your `ui-lovelace.ysml`.

- Find your `mini-media-player.js` file in `config/www` or wherever you ended up storing it.
- Replace the file with the latest version of [mini-media-player.js](mini-media-player.js).
- Add the new version number to the end of the cards reference url in your `ui-lovelace.yaml` like below.

```yaml
resources:
  - url: /local/mini-media-player.js?v=0.8.9
    type: module
```

If you went the `git clone` route, just run `git pull` from inside your `config/www/mini-media-player` directory, to get the latest version of the code.

*You may need to empty the browsers cache if you have problems loading the updated card.*

## Using the card

### Options

| Name | Type | Default | Since | Description |
|------|:----:|:-------:|:-----:|-------------|
| type | string | **required** | v0.1 | `custom:mini-media-player`
| entity | string | **required** | v0.1 | An entity_id from an entity within the `media_player` domain.
| title | string | optional | v0.1 | Set a card title.
| name | string | optional | v0.6 | Override the entities friendly name.
| icon | string | optional | v0.1 | Specify a custom icon from any of the available mdi icons.
| group | boolean | false | v0.1 | Disable paddings and box-shadow.
| show_tts | string | optional | v0.2 | Show Text-To-Speech input, specify [TTS platform](https://www.home-assistant.io/components/tts/), e.g. `show_tts: google` or `show_tts: amazon_polly`.
| show_source | string | false | v0.7 | `true` to display source select, `small` to display the source button & hide current source (v0.8.1).
| show_progress | boolean | false | v0.8.3 | Display a progress bar when media progress information is available.
| show_shuffle | boolean | false | v0.8.9 | Display a shuffle button (only for players with `shuffle_set` support).
| hide_power | boolean | false | v0.7 | Hide the power button.
| hide_controls | boolean | false | v0.8 | Hide media control buttons (*force `short_info` to `true`*).
| hide_volume | boolean | false | v0.8 | Hide volume controls. (*force `short_info` to `true`*).
| hide_mute | boolean | false | v0.8.1 | Hide the mute button.
| hide_info | boolean | false | v0.8.4 | Hide entity icon, entity name & media information.
| hide_icon | boolean | false | v0.8.8 | Hide the entity icon.
| artwork | string | default | v0.4 | `cover` to have artwork displayed as the card background, `none` to hide artwork.
| short_info | boolean | false | v0.8 | Limit media information to one row.
| scroll_info | boolean | false | v0.8 | Limit media information to one row, scroll through the overflow.
| power_color | boolean | false | v0.4 | Make power button change color based on on/off state.
| artwork_border | boolean | false | v0.3 | Display a border around the `default` media artwork.
| volume_stateless | boolean | false | v0.6 | Swap out the volume slider for volume up & down buttons.
| toggle_power | boolean | true | v0.8.9 | Change power button behaviour `turn_off` / `turn_on` or `toggle`
| consider_idle_after | number | optional | v0.8.9 | Specify a number (minutes) *only supported on players with `media_position_updated_at`)* after which the player displays as idle.
| more_info | boolean | true | v0.1 | Enable the "more info" dialog when pressing on the card.
| max_volume | number | true | v0.8.2 | Max volume for the volume slider (number between 1 - 100).
| background | string | optional | v0.8.6 | Background image, specify the image url `"/local/background-img.png"` e.g.

### Example usage

#### Single player
```yaml
- type: custom:mini-media-player
  entity: media_player.avr
  icon: mdi:router-wireless
  artwork_border: true
  show_source: true
```

<img src="https://user-images.githubusercontent.com/457678/47515832-256d4180-d884-11e8-97a6-267c5c63c000.png" width="500px" />

#### Compact player
Setting either `hide_volume` and/or `hide_controls` to `true` will make the  player collapse into one row.
```yaml
- type: custom:mini-media-player
  entity: media_player.spotify
  name: Spotify Player
  artwork: cover
  power_color: true
  hide_volume: true
  show_progress: true
```

<img src="https://user-images.githubusercontent.com/457678/47516141-fc997c00-d884-11e8-9bc9-eb9b0818f28b.png" width="500px" />

#### Grouping players
Set the `group` option to `true` when nesting the mini media player(s) inside  cards that already have margins/paddings.

```yaml
- type: entities
  title: Media Players
  entities:
    - entity: media_player.spotify
      type: custom:mini-media-player
      group: true
    - entity: media_player.google_home
      type: custom:mini-media-player
      hide_controls: true
      show_tts: google
      group: true
    - entity: media_player.samsung_tv
      type: custom:mini-media-player
      hide_controls: true
      group: true
```

<img src="https://user-images.githubusercontent.com/457678/47516557-08d20900-d886-11e8-8922-4973c0aab94a.png" width="500px" />

## Bundle

If you want to avoid loading resources from https://unpkg.com, you can use
`mini-media-player-bundle.js` instead.

## Updating bundle (for developer)

Install `nodejs` and `npm`, install dependances by running:

```
$ npm install
```

Edit source file `mini-media-player.js`, build by running:
```
$ npm build
```

Then `mini-media-player-bundle.js` will be updated and ready to use.

Finally commit file `mini-media-player-bundle.js` to git.

## Getting errors?
Make sure you have `javascript_version: latest` in your `configuration.yaml` under `frontend:`.

Make sure you have the latest version of `mini-media-player.js`.

If you have issues after updating the card, try clearing your browsers cache or restart Home Assistant.

## Inspiration
- [@ciotlosm](https://github.com/ciotlosm) - [custom-lovelace](https://github.com/ciotlosm/custom-lovelace)
- [@c727](https://github.com/c727) - [Custom UI: Mini media player](https://community.home-assistant.io/t/custom-ui-mini-media-player/40135)

## License
This project is under the MIT license.
