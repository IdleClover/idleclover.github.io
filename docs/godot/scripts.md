---
title: Scripts
layout: default
parent: Godot
nav_order: 1
---

# Scripts

Différents scripts utiles pour la gestion d'un projet Godot

## Nouveau projet


<div class="language-bash highlighter-rouge"><div class="highlight" tabindex="0"><pre class="highlight"><code id="godot-new-project">git fetch upstream 4.7 <span class="c"># Mettre à jour ou installer une nouvelle version</span>
git switch <span class="nt">-c</span> &lt;project-name&gt; upstream/4.7
git push <span class="nt">-u</span> origin &lt;project-name&gt;
</code></pre></div><button type="button" aria-label="Copy code to clipboard"><svg viewBox="0 0 24 24" class="copy-icon"><use xlink:href="#svg-copy"></use></svg></button></div>

<div class="git-config">
  <label>Project : <input id="project-name" type="text" placeholder="<project-name>"/></label>
  <label>Version :
    <select class="version-select" id="new-project-version">
      <option value="4.7">4.7</option>
      <option value="4.6">4.6</option>
    </select>
  </label>
</div>

## Mettre à jour

Généralement, on veut rester sur la même version tout du long, mais si c'est juste pour avoir le dernier patch, c'est raisonnable.

### Sans modifications

Le moteur n'a pas encore été modifié, donc on reprend juste les deux derniers commits.

<div class="language-bash highlighter-rouge"><div class="highlight" tabindex="0"><pre class="highlight"><code id="godot-without">git fetch upstream 4.7
git reset <span class="nt">--hard</span> upstream/4.7
</code></pre></div><button type="button" aria-label="Copy code to clipboard"><svg viewBox="0 0 24 24" class="copy-icon"><use xlink:href="#svg-copy"></use></svg></button></div>

<div class="git-config">
  <label>Version :
    <select class="version-select" id="update-without-project-version">
      <option value="4.7">4.7</option>
      <option value="4.6">4.6</option>
    </select>
  </label>
</div>

### Avec modifications

Le moteur a été modifié, il faut donc reprendre tous les commits intermédiaires.

<div class="language-bash highlighter-rouge"><div class="highlight" tabindex="0"><pre class="highlight"><code id="godot-with">git fetch upstream 4.7
git rebase upstream/4.7
</code></pre></div><button type="button" aria-label="Copy code to clipboard"><svg viewBox="0 0 24 24" class="copy-icon"><use xlink:href="#svg-copy"></use></svg></button></div>

<div class="git-config">
  <label>Version :
    <select class="version-select" id="update-with-project-version">
      <option value="4.7">4.7</option>
      <option value="4.6">4.6</option>
    </select>
  </label>
</div>


<script>
function updateNewProjectSnippet() {
    const name = document.getElementById('project-name').value.toLowerCase().replaceAll(" ", "-") || '&lt;project-name&gt';
    const version = document.getElementById('new-project-version').value;

    document.getElementById('godot-new-project').innerHTML =
`git fetch upstream ${version} <span class="c"># Mettre à jour ou installer une nouvelle version</span>\ngit switch <span class="nt">-c</span> ${name} upstream/${version}\ngit push <span class="nt">-u</span> origin ${name}\n`;
}


function updateUpdateProjectSnippet() {
    const version = document.getElementById('update-without-project-version').value;

    document.getElementById('godot-without').innerHTML =
`git fetch upstream ${version}\ngit reset <span class="nt">--hard</span> upstream/${version}`;


    document.getElementById('godot-with').innerHTML =
`git fetch upstream ${version}\ngit rebase upstream/${version}`;
}


async function fetchGodotVersions() {
    const response = await fetch(
        'https://api.github.com/repos/godotengine/godot/branches'
    );
    const branches = await response.json();
    return branches.map(b => b.name).filter(name => name != "master").reverse();
}

async function populateVersionSelect() {
    const versions = await fetchGodotVersions();
    for (const select of document.getElementsByClassName('version-select')) {
        select.innerHTML = versions
            .map(v => `<option value="${v}">${v}</option>`)
            .join('');
    }
    
    updateNewProjectSnippet();
}

populateVersionSelect();

document.getElementById('project-name').addEventListener('input', updateNewProjectSnippet);
document.getElementById('new-project-version').addEventListener('change', updateNewProjectSnippet);

const updateWithout = document.getElementById('update-without-project-version');
const updateWith = document.getElementById('update-with-project-version');

updateWithout.addEventListener('change', () => {
    updateWith.value = updateWithout.value;
    updateUpdateProjectSnippet();
});
updateWith.addEventListener('change', () => {
    updateWithout.value = updateWith.value;
    updateUpdateProjectSnippet();
});
</script>