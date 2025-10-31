This is an example of my version of the nostromo_crt theme originally created here:
https://github.com/Andrew504s/weyland-home-assistant-theme/tree/main

I have made several changes to the original theme and I am providing this code purely as an example.  This comes with no support / install instuctions etc.

In particular, I have tried to put as much of the card_mod styling into the theme as possible.
<img width="1121" height="687" alt="chrome_NBN11wkNNC" src="https://github.com/user-attachments/assets/b86e9a85-5051-4044-bf2e-f12bd74cbafe" />

All my code has been 'vibe-coded' using a combination of Grok, Gemini and Chat-GPT.

Example Light / toggle button card code:
<pre>``` 
show_name: true
show_icon: false
type: button
entity: group.kitchen
name: KITCHEN
show_state: false
grid_options:
  columns: 6
  rows: 1
tap_action:
  action: toggle
card_mod:
  class: nostromo-base nostromo-button nostromo-kitchen-base
  style: |
    ha-card::before {
      background:
        {% if is_state(config.entity, 'on') %}
          var(--g, #00ff7a)
        {% else %}
          transparent
        {% endif %};
    }
    ha-card::after {
      content: "{{ (states('sensor.kitchen_motion_temperature') | float(0) | round(1, 'half') ) }}°C";
    }
  ```</pre>


Example Markup:</br>
Note - I found that my tablet would crash when I had a flashing CSS cursor that animated forever.  As such, I replaced the CSS cursor found in the original code with a .gif that plays on loop.
<pre> ```
type: markdown
text_only: true
entity_id:
  - sensor.time
  - person.person1
  - person.person2
  - person.person3
  - alarm_control_panel.alarmo
  - sensor.bin_day_sensor
content: >
  USCSS 23DC  |  DATE: {{ now().strftime('%d%m%y') }}  |  CREW: {{
  expand('person.person1','person.person2','person.person3')
     | selectattr('state','eq','home') | list | length }}
  NEXT BIN: {{ state_attr('sensor.bin_day_sensor', 'bin_type') | upper }} {{
  states('sensor.bin_day_sensor') }} |  </br>SECURITY: {{
  states('alarm_control_panel.alarmo') | upper }}
card_mod:
  style: |
    @media (prefers-reduced-motion: reduce) {
      ha-card ha-markdown::after { animation: none !important; }
    }

    ha-card {
      --g: var(--nostromo-green, #00ff7a);
      --a: var(--nostromo-amber, #ffb000);
      background: transparent;
      border: none;
      padding: 0 12px !important;
      font-family: var(--nostromo-font, ui-monospace, SFMono-Regular, Menlo, Consolas, "Liberation Mono", monospace);
      text-transform: uppercase;
      color: var(--g);
      letter-spacing: 2px;
      font-size: 18px !important;
      white-space: normal;
      overflow-wrap: anywhere;
    }

    /* pulse effect instead of blink */
    @keyframes nostromo-pulse {
      0%, 100% { text-shadow: 0 0 0 rgba(0, 0, 0, 0); }
      50%      { text-shadow: 0 0 6px currentColor; }
    }

    ha-card ha-markdown::after {
      content: "\200B";
      display: inline-block;
      vertical-align: middle;
      width: 10px;
      background-size: 10px 10px; 
      background: url('/local/nostromo_cursor3.gif') no-repeat center center;
    }
  
  
  ``` </pre>
