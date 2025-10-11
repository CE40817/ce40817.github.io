---
layout: default
title: Course Modules
nav_order: 2
permalink: /asn/modules/
---

{% comment %}
Lookup course data from _data. Preferred path: _data/asn/course.yml (or .yaml).
Fallback: _data/course.yml (or .yaml).
The expected structure matches the example you shared:
title, quarter, modules: [...], weeks: [...], agenda: [...]
{% endcomment %}
{% assign course = site.data.asn.course | default: site.data.course %}

{% if course == nil %}
<p><em>Course data not found.</em> Please create one of these files:</p>
<ul>
  <li><code>_data/asn/course.yml</code> (preferred)</li>
  <li><code>_data/asn/course.yaml</code></li>
  <li><code>_data/course.yml</code></li>
  <li><code>_data/course.yaml</code></li>
</ul>
<p>Expected keys include <code>title</code>, <code>quarter</code>, <code>modules</code>, and <code>agenda</code>.</p>
{% else %}

<style>
:root{--border:#d9dee3; --muted:#6b7280; --ink:#0f172a; --bg:#ffffff; --bg-alt:#f8fafc; --ring:#e2e8f0;}
.main-content h1{font-weight:800; letter-spacing:.1px;}
.main-content p.lead{color:var(--muted); margin-top:-.4rem;}
.modules-grid{display:grid; grid-template-columns:repeat(auto-fit, minmax(320px, 1fr)); gap:1rem; margin:1rem 0 1.5rem; align-items:stretch; grid-auto-rows:1fr;}
.mod-card{position:relative; background:var(--bg); border:1px solid var(--border); border-radius:14px; padding:1rem 1rem .9rem; display:flex; flex-direction:column; gap:.55rem; height:100%; color:var(--ink); text-decoration:none; transition:box-shadow .18s ease, border-color .18s ease, transform .08s ease; box-shadow:0 1px 0 var(--ring) inset;}
.mod-card:hover{border-color:#cbd5e1; box-shadow:0 2px 14px rgba(0,0,0,.06), 0 1px 0 var(--ring) inset; transform:translateY(-1px);}
.mod-card::before{content:""; position:absolute; left:0; right:0; top:0; height:4px; border-top-left-radius:14px; border-top-right-radius:14px; background:linear-gradient(90deg, var(--g1), var(--g2));}
.mod-top{display:flex; align-items:center; justify-content:space-between; gap:.75rem;}
.mod-badge{display:inline-flex; align-items:center; gap:.5rem; padding:.35rem .6rem; border-radius:999px; font-weight:700; font-size:.88rem; background:linear-gradient(90deg, var(--g1), var(--g2)); color:#0b1220; background-clip:padding-box; box-shadow:inset 0 0 0 1px rgba(255,255,255,.55);}
.mod-sessions{font-size:.8rem; color:var(--muted);}
.mod-title{font-weight:800; line-height:1.2; margin:.2rem 0 .1rem;}
.mod-focus{margin:.1rem 0 0; padding-left:1.2rem;}
.mod-focus li{margin:.18rem 0;}
.mod-focus li::marker{color:#475569;}
.mod-outcome{margin-top:auto; background:linear-gradient(180deg, var(--bg-alt), #ffffff); border:1px dashed var(--border); border-left:3px solid var(--g1); padding:.55rem .7rem; border-radius:10px;}
.mod-outcome strong{font-weight:800;}
.caption{color:var(--muted); font-size:.98rem; margin-top:-.45rem;}
.header-meta{color:var(--muted); font-size:.95rem; margin:.2rem 0 .8rem;}
</style>

{% assign modules = course.modules | default: empty %}
{% assign agenda  = course.agenda  | default: empty %}
{% assign nmods = modules | size %}
{% assign nsessions = agenda | size %}

# High-Level Modules
<div class="header-meta">
  {{ course.title | default: "Course" }}{% if course.quarter %} · {{ course.quarter }}{% endif %}
  · {{ nmods }} modules · {{ nsessions }} sessions total
</div>
<p class="lead">Each module is typically delivered across two sessions; the badge shows the actual count based on the schedule.</p>

<div class="modules-grid">
  {% for m in modules %}
    {% assign m_no   = m.mod_no  | default: m.no   | default: forloop.index %}
    {% assign m_id   = m.mod_id  | default: m.id   | default: "m" | append: m_no %}
    {% assign m_name = m.name    | default: m.title %}
    {% assign m_out  = m.outcome %}

    {%- comment -%}
    Count sessions for this module from course.agenda where mod_id == m_id
    {%- endcomment -%}
    {% assign sess_for_mod = agenda | where: "mod_id", m_id %}
    {% assign sess_count = sess_for_mod | size %}
    {% if sess_count == 0 %}{% assign sess_label = "—" %}{% else %}{% assign sess_label = sess_count | append: " session" %}{% if sess_count != 1 %}{% assign sess_label = sess_label | append: "s" %}{% endif %}{% endif %}

    {% assign hue = forloop.index0 | times: 29 %}
    {% assign h1 = hue | modulo: 360 %}
    {% assign h2 = hue | plus: 35 | modulo: 360 %}

    <a class="mod-card" id="{{ m_id }}" href="#{{ m_id }}" style="--g1: hsl({{ h1 }}, 75%, 58%); --g2: hsl({{ h2 }}, 75%, 52%);" aria-label="Module {{ m_no }}: {{ m_name }}">
      <div class="mod-top">
        <span class="mod-badge">Module {{ m_no }}</span>
        <span class="mod-sessions">{{ sess_label }}</span>
      </div>
      <div class="mod-title">{{ m_name }}</div>

      {% if m.focus %}
        {%- comment -%}
        Your course.yaml uses a single-line string for "focus".
        If you later switch to a YAML list, replace the paragraph with a UL.
        {%- endcomment -%}
        {% if m.focus.first and m.focus | join: "" != m.focus %}
          <ul class="mod-focus">
            {% for f in m.focus %}
              <li>{{ f }}</li>
            {% endfor %}
          </ul>
        {% else %}
          <p class="caption">{{ m.focus }}</p>
        {% endif %}
      {% endif %}

      {% if m_out %}
        <div class="mod-outcome"><strong>Outcome:</strong> {{ m_out }}</div>
      {% endif %}
    </a>
  {% endfor %}
</div>

{% endif %}
