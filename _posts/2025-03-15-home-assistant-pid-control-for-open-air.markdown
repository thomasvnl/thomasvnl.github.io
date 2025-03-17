---
layout: post
title:  "Home Assistant PID Control for OpenAIR"
date:   2025-03-15 14:10:28 +0100
categories: OpenAIR Mechanical-Ventilation
---

PID Controller implementatie
- Aanmaken van helpers
    - Aanmaken van input_numbers -> Uitleggen dat het handmatig kan via configuration.yaml of via Settings -> Devices & Services -> Helpers -> Create Helper
    - Gebruik maken van PID controller van [https://github.com/soloam/ha-pid-controller](https://github.com/soloam/ha-pid-controller)
    - 

{% highlight yaml %}
{% raw %}
sensor:
  - platform: pid_controller
    name: ventilatie_co2_pid_controller
    unique_id: '7b358144-007e-4e9d-b6d5-b811c37deba4'
    enabled: '{{ is_state("input_select.ventilatie_stand", "automatisch") }}'
    set_point: '{{ states("input_number.ventilatie_gewenst_co2") }}'
    p: '{{ states("input_number.ventilatie_pid_proportional") }}'
    i: '{{ states("input_number.ventilatie_pid_integral") }}'
    d: '{{ states("input_number.ventilatie_pid_derivative") }}'
    entity_id: sensor.maximum_gemeten_co2_huis
    invert: yes
    precision: 0
    sample_time: '{{ states("input_number.ventilatie_pid_sample_time") }}'
    minimum: '{{ states("input_number.ventilatie_automatisch_min_snelheid") }}'
    maximum: '{{ states("input_number.ventilatie_automatisch_max_snelheid") }}'
    windup: '{{ states("input_number.ventilatie_pid_windup") }}'
    round: Round
    unit_of_measurement: '%'
{% endraw %}
{% endhighlight %}

{% highlight yaml %}
input_number:
  ventilatie_pid_sample_time:
    name: "Ventilatie PID snelheid van output waarden generatie"
    min: 10
    max: 300
    step: 10
    icon: 'mdi:timer-cog-outline'
    unit_of_measurement: 's'
  ventilatie_automatisch_min_snelheid:
    name: "Ventilatie Minimum Snelheid Automatische Stand"
    min: 10
    max: 40
    step: 5
    icon: 'mdi:speedometer-slow'
    unit_of_measurement: '%'
  ventilatie_automatisch_max_snelheid:
    name: "Ventilatie Maximum Snelheid Automatische Stand"
    min: 60
    max: 100
    step: 5
    icon: 'mdi:speedometer'
    unit_of_measurement: '%'
  ventilatie_gewenst_co2:
    name: "Gewenst CO2 niveau"
    min: 420
    max: 1500
    step: 10
    icon: 'mdi:molecule-co2'
    unit_of_measurement: 'ppm'
  ventilatie_pid_proportional:
    name: "Ventilatie PID proportional"
    min: 0
    max: 10
    step: 0.01
    icon: 'mdi:math-integral'
  ventilatie_pid_integral:
    name: "Ventilatie PID integral"
    min: 0
    max: 0.001
    step: 0.00001
    icon: 'mdi:math-integral'
  ventilatie_pid_derivative:
    name: "Ventilatie PID derivative"
    min: 0
    max: 0.001
    step: 0.00001
    icon: 'mdi:math-integral'
  ventilatie_pid_windup:
    name: "Ventilatie PID Windup"
    min: 0
    max: 100
    step: 0.5
    icon: 'mdi:car-speed-limiter'


{% endhighlight %}