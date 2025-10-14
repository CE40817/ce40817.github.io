---
layout: default
title: Course Page
nav_order: 1
---

# **CE 40-817: Advanced Network Security**
### **Sharif University of Technology**
## Fall 2025

**Instructor**  
Solmaz Salimi, <span class="email-obf">s[dot][salimi][at][sharif][dot][edu]</span>

**TA(s)**  
Kasra Abdollahi,  <span class="email-obf">kasra[dot][abdolahi1379][at][sharif][dot][edu]</span>

**Lectures**  
Sunday/Tuesday 10:30 to 12:00, Room **402**
Join the Discord server for announcements: <a href="https://discord.gg/WgW5SeHQ" target="_blank" rel="noopener">discord.gg/WgW5SeHQ</a>

## Course Description
<hr>
- This course explores how to secure modern networks at Internet scale.
- Each week features a short **Recent Trends and AI Angle** spotlight.

## Course Schedule
<hr>

<div class="schedule-table">
  {% for week in site.data.course.weeks %}
    <div class="col">
      <div class="head">{{ week.label }}</div>
      {% for d in week.dates %}
        <div class="cell"><a href="#{{ d.id }}">{{ d.label }}</a></div>
      {% endfor %}
    </div>
  {% endfor %}
</div>

<hr>
<div class="agenda">
  {% for w in site.data.course.weeks %}
    {% for d in w.dates %}
      {% assign a = site.data.course.agenda | where: "id", d.id | first %}
      {% if a %}
        {% assign idx = a.mod_no | minus: 1 %}
        {% assign hue = idx | times: 29 %}
        {% assign h1  = hue | modulo: 360 %}
        {% assign h2  = hue | plus: 35 | modulo: 360 %}
      <div class="slot" id="{{ a.id }}">
        <div class="leftcol">
          <div class="datebox" style="--g1:hsl({{ h1 }},75%,58%);--g2:hsl({{ h2 }},75%,52%);">
            <div class="dow">{{ a.dow }}</div>
            <div class="md">{{ a.md }}</div>
          </div>
        </div>
        <div>
          <div class="topline">
            {% if a.title %}
              <a class="mod-badge"
                 href="/asn/modules/#{{ a.mod_id }}"
                 style="--g1:hsl({{ h1 }},75%,58%);--g2:hsl({{ h2 }},75%,52%);--num-color:hsl({{ h2 }},95%,85%);">
                {{ a.module }}:
              </a>
              <strong class="title">{{ a.title }}</strong>
              {% if a.slides %}
                <a class="mod-badge"
                   href="{{ a.slides }}"
                   style="--g1:hsl({{ h1 }},75%,58%);--g2:hsl({{ h2 }},75%,52%);--num-color:hsl({{ h2 }},95%,85%);">
                  [Slides]
                </a>
              {% else %}
                <span class="mod-badge is-disabled"
                      aria-disabled="true"
                      style="--g1:hsl({{ h1 }},0%,70%);--g2:hsl({{ h2 }},0%,60%);--num-color:hsl({{ h2 }},0%,90%);">
                  [TBU]
                </span>
              {% endif %}
            {% endif %}
          </div>

          {% if a.milestone %}
            <div class="callout {{ a.milestone.type }}{% if a.milestone.full %} full{% endif %}">
              <span class="tag">
                {% case a.milestone.type %}
                  {% when "release" %} Assignment Release
                  {% when "due"     %} Assignment Due
                  {% when "exam"    %} Exam
                  {% else           %} Note
                {% endcase %}
              </span>
              <span class="text">{{ a.milestone.text }}</span>
            </div>
          {% endif %}

          <div class="meta">
            {% if a.topic %}
              <details class="row topic"{% if a.topic_open %} open{% endif %}>
                <summary><span class="label">Topic</span></summary>
                <div class="val">
                  {% assign tjson = a.topic | jsonify %}
                  {% if tjson contains '[' %}
                    <ul class="tight">
                      {% for t in a.topic %}
                        <li>{{ t | markdownify | strip | replace: '<p>', '' | replace: '</p>', '' }}</li>
                      {% endfor %}
                    </ul>
                  {% else %}
                    {{ a.topic | markdownify }}
                  {% endif %}
                </div>
              </details>
            {% endif %}

            {% if a.ai_angel %}
              <details class="row ai_angel"{% if a.ai_open %} open{% endif %}>
                <summary><span class="label">AI Angle</span></summary>
                <div class="val">
                  {% assign aijson = a.ai_angel | jsonify %}
                  {% if aijson contains '[' %}
                    <ul class="tight">
                      {% for x in a.ai_angel %}
                        <li>{{ x | markdownify | strip | replace: '<p>', '' | replace: '</p>', '' }}</li>
                      {% endfor %}
                    </ul>
                  {% else %}
                    {{ a.ai_angel | markdownify }}
                  {% endif %}
                </div>
              </details>
            {% endif %}

            {% if a.optional and a.optional.size > 0 %}
              <details class="row optional"{% if a.opt_open %} open{% endif %}>
                <summary><span class="label">Optional Papers</span></summary>
                <div class="val">
                  <ol class="tight refs">
                    {% for o in a.optional %}
                      <li>{{ o | markdownify | strip | replace: '<p>', '' | replace: '</p>', '' }}</li>
                    {% endfor %}
                  </ol>
                </div>
              </details>
            {% endif %}
          </div>
        </div>
      </div>
      {% endif %}
    {% endfor %}
  {% endfor %}
</div>
