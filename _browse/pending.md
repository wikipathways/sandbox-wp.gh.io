---
layout: default
order: 4
display-title: "Pending"
tooltip: "Pathway drafts currently in review" 
btn-class: "btn-front"
---
    
<h2 id="title">Draft Pathways</h2>
<p>A date-sorted list of pathways with new edits currently in review. </p> 
<a class="btn btn-sm btn-front my-2" style="float:right;margin-right:20px;" title="Browse recently updated pathways" href="/browse/updated.html">Updated</a><a class="btn btn-sm btn-front my-2" style="float:right;margin-right:20px;" title="Browse newly added pathways" href="/browse/new.html">New</a>

<h2>Pathways</h2>
<ul class="nav nav-tabs">
    <li class="nav-item">
        <a class="nav-link active" data-toggle="tab" href="#gallery">Gallery</a>
    </li>
      <li class="nav-item">
        <a class="nav-link" data-toggle="tab" href="#list">List</a>
      </li>
</ul>
{% assign draft_pathways = site.drafts | sort: "last-edited" | reverse %}
<div class="tab-content" >
    <div class="tab-pane fade show active" id="gallery" role="tabpanel">
        <br/>
    <div class="row">
      {% for pw in draft_pathways %}
          <div class="col-sm-auto">
            <div class="card" style="width: 10rem;">
              <a class="card-link" href="{{ pw.url }}">
              <img class="card-img-top" loading="lazy" src="/draft_assets/{{pw.wpid}}/{{pw.wpid}}.png" alt="{{ pw.title }}">
              <div class="card-body">
                <p class="card-text">{{ pw.title }} <em>({{ pw.organisms.first }})</em>
                <br /><b>last edited: {{ pw.last-edited }}</b></p>
              </div>
              </a>
            </div>
          </div>
      {% endfor %}
    </div>
  </div>
  <div class="tab-pane fade" id="list" role="tabpanel">
    <br/>
    <div class="row" style="margin-left: 10px;">
      <ul>
        {% for pw in draft_pathways %}
              <li><a href="{{ pw.url }}">{{ pw.title }} - {{pw.wpid}} <em>({{ pw.organisms.first }})</em> --<b>last edited: {{ pw.last-edited }}</b></a></li>
        {% endfor %}
      </ul>
    </div>  
  </div>
</div>
<br/>
<hr/>
<h2>Authors of Pathways with Drafts in Review</h2>
<!-- AUTHOR PROCESSING -->
{% assign blocked_authors = "Wpblocked,Unknown,TestUser,MaintBot" | split:"," %}
{% assign sorted_pw_authors = draft_pathways | map: "authors" | join: ','  | split: ',' | uniq  %} <!-- REPLACE authors with "recent author" -->
{% assign all_authors = '' | split: '' %}
{% for auth in sorted_pw_authors %}
  {% unless blocked_authors contains auth %}
    {% assign realname = site.authors | where: "username", auth | map: "realname" | first  %}
    {% unless blocked_authors contains realname %}
      {% capture thisAuth %}
        <a href="{{site.url}}/authors/{{auth}}.html" title="View author profile">{{realname}}</a>
      {% endcapture %}
      {% assign all_authors = all_authors | push: thisAuth %}
    {% endunless %}
  {% endunless %}
{% endfor %}
<p>{{ all_authors | uniq | array_to_sentence_string }}.</p>
