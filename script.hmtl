// script.js
(() => {
  const $ = (q, el=document) => el.querySelector(q);
  const $$ = (q, el=document) => Array.from(el.querySelectorAll(q));

  // ===== Theme =====
  const THEME_KEY = "dei_theme";
  const root = document.documentElement;
  const themeBtn = $("#themeBtn");

  function setTheme(mode){
    if(mode === "light") root.setAttribute("data-theme","light");
    else root.removeAttribute("data-theme");
    localStorage.setItem(THEME_KEY, mode);
  }

  const savedTheme = localStorage.getItem(THEME_KEY);
  if(savedTheme) setTheme(savedTheme);
  else {
    // Respect OS preference by default
    const prefersLight = window.matchMedia && window.matchMedia("(prefers-color-scheme: light)").matches;
    setTheme(prefersLight ? "light" : "dark");
  }

  themeBtn.addEventListener("click", () => {
    const cur = root.getAttribute("data-theme") === "light" ? "light" : "dark";
    setTheme(cur === "light" ? "dark" : "light");
    toast(`Theme: ${cur === "light" ? "Dark" : "Light"}`);
  });

  // ===== Data (mock catalog) =====
  // NOTE: No actual audio file—this is a UI mock. “Playback” is simulated.
  const ARTIST = "Dei";
  const catalog = [
    { id:"t1", title:"Night Frequency", artist:ARTIST, album:"Signal Room", dur: "3:12", mood:["Underground","Cosmic","Late Night"], heat: "Hot" },
    { id:"t2", title:"Clean Cuts", artist:ARTIST, album:"Signal Room", dur: "2:48", mood:["Clarity","Bars","Focus"], heat: "New" },
    { id:"t3", title:"Neon Discipline", artist:ARTIST, album:"Midnight Doctrine", dur: "3:41", mood:["Grind","Precision","Cold"], heat: "Top" },
    { id:"t4", title:"Pressure Test", artist:ARTIST, album:"Midnight Doctrine", dur: "2:59", mood:["Intensity","Edge","Drive"], heat: "Top" },
    { id:"t5", title:"Noisy Silence", artist:ARTIST, album:"Deep Cuts", dur: "4:03", mood:["Conscious","Weight","Dark"], heat: "Deep" },
    { id:"t6", title:"Satellite Streets", artist:ARTIST, album:"Deep Cuts", dur: "3:26", mood:["Cruise","Atmosphere","Smoke"], heat: "New" },
    { id:"t7", title:"Blueprint Bars", artist:ARTIST, album:"Work Ethic", dur: "2:33", mood:["Focus","Craft","Structure"], heat: "Top" },
    { id:"t8", title:"Under the Overhang", artist:ARTIST, album:"Work Ethic", dur: "3:08", mood:["Underground","Grit","Soul"], heat: "Deep" }
  ];

  const releases = [
    { id:"r1", title:"Signal Room (EP)", sub:"8 tracks • cosmic precision", ref:"t1" },
    { id:"r2", title:"Midnight Doctrine", sub:"hard edges • clean mix", ref:"t3" },
    { id:"r3", title:"Deep Cuts", sub:"conscious • heavy air", ref:"t5" },
    { id:"r4", title:"Work Ethic", sub:"craft • discipline", ref:"t7" },
  ];

  const mixes = [
    { id:"m1", title:"Late Night Code", sub:"focus / flow / silence", badge:"Mix", seed:["t1","t2","t7","t6"] },
    { id:"m2", title:"Fight Energy", sub:"edge / pressure / drive", badge:"Mix", seed:["t4","t3","t1","t8"] },
    { id:"m3", title:"Cosmic Drift", sub:"atmosphere / neon / smoke", badge:"Mix", seed:["t6","t1","t5","t2"] },
    { id:"m4", title:"Clean Bars Only", sub:"craft / clarity / punch", badge:"Mix", seed:["t7","t2","t3","t4"] },
  ];

  const browseTiles = [
    { title:"Underground", desc:"Raw energy, no polish-for-clout.", badge:"Vibe" },
    { title:"Cosmic", desc:"Space in the mix. Heavy air.", badge:"Vibe" },
    { title:"Bars", desc:"Craft first. Word weight matters.", badge:"Focus" },
    { title:"Focus Mode", desc:"Low distraction, high output.", badge:"Utility" },
    { title:"Night Drives", desc:"Dark roads, bright thoughts.", badge:"Scene" },
    { title:"Instrumentals", desc:"Room for your own noise.", badge:"Session" }
  ];

  const rooms = [
    { title:"The Listening Room", desc:"Editorial picks — clean, cosmic, lethal.", badge:"Editorial" },
    { title:"Deep Cuts Archive", desc:"Underrated tracks with real weight.", badge:"Editorial" },
    { title:"Neon Workshop", desc:"Rough drafts, sharp edges, cold truth.", badge:"Studio" },
    { title:"Quiet Pressure", desc:"Conscious cuts that don’t beg to be heard.", badge:"Editorial" }
  ];

  const stations = [
    { title:"Night Frequency", desc:"Underground / cosmic / late", badge:"Station", seed:["t1","t6","t5","t2"] },
    { title:"Grind Mode", desc:"Discipline / craft / focus", badge:"Station", seed:["t7","t2","t3","t4"] },
    { title:"Pressure", desc:"Intensity / edge / drive", badge:"Station", seed:["t4","t3","t8","t1"] },
    { title:"Deep Air", desc:"Conscious / heavy / dark", badge:"Station", seed:["t5","t1","t6","t8"] }
  ];

  // ===== Elements =====
  const pages = {
    listen: $("#page-listen"),
    browse: $("#page-browse"),
    radio: $("#page-radio"),
    library: $("#page-library"),
    search: $("#page-search"),
  };

  const tabBtns = $$(".tabbar .tab");
  const pageTitle = $("#pageTitle");
  const pageSub = $("#pageSub");

  const releaseRail = $("#releaseRail");
  const mixGrid = $("#mixGrid");
  const topList = $("#topList");
  const browseGrid = $("#browseGrid");
  const roomsGrid = $("#roomsGrid");
  const stationGrid = $("#stationGrid");
  const libraryPane = $("#libraryPane");

  const searchInput = $("#searchInput");
  const clearSearch = $("#clearSearch");
  const resultsList = $("#resultsList");
  const resultsMeta = $("#resultsMeta");

  // Mini player
  const miniPlayer = $("#miniPlayer");
  const miniCover = $("#miniCover");
  const miniTitle = $("#miniTitle");
  const miniArtist = $("#miniArtist");
  const miniBar = $("#miniBar");

  const prevBtn = $("#prevBtn");
  const playBtn = $("#playBtn");
  const nextBtn = $("#nextBtn");
  const playIcon = $("#playIcon");

  // Full player
  const overlay = $("#playerOverlay");
  const openPlayer = $("#openPlayer");
  const closePlayer = $("#closePlayer");
  const playerCover = $("#playerCover");
  const playerTitle = $("#playerTitle");
  const playerArtist = $("#playerArtist");
  const seek = $("#seek");
  const tCur = $("#tCur");
  const tDur = $("#tDur");
  const playerPrev = $("#playerPrev");
  const playerNext = $("#playerNext");
  const playerPlay = $("#playerPlay");
  const shuffleBtn = $("#shuffleBtn");
  const repeatBtn = $("#repeatBtn");
  const vol = $("#vol");
  const queueList = $("#queueList");
  const clearQueue = $("#clearQueue");
  const regenVibe = $("#regenVibe");
  const vibeTags = $("#vibeTags");

  // CTA buttons
  const playFeatured = $("#playFeatured");
  const shuffleAll = $("#shuffleAll");
  const startRadio = $("#startRadio");
  const radioInfo = $("#radioInfo");

  // ===== UI Builders =====
  function makeCover(el, seed){
    // deterministic-ish gradient based on seed
    let h = 0;
    for(const ch of seed) h = (h * 31 + ch.charCodeAt(0)) >>> 0;
    const a = (h % 360);
    const b = (a + 110) % 360;
    const c = (a + 220) % 360;
    el.style.background = `linear-gradient(135deg,
      hsla(${a}, 92%, 60%, .18),
      hsla(${b}, 92%, 62%, .16),
      hsla(${c}, 92%, 58%, .12)
    )`;
  }

  function cardRelease(r){
    const track = catalog.find(x => x.id === r.ref) || catalog[0];
    const el = document.createElement("div");
    el.className = "card";
    el.innerHTML = `
      <div class="cover" aria-hidden="true"></div>
      <div class="card-body">
        <div class="title">${escapeHtml(r.title)}</div>
        <div class="sub">${escapeHtml(r.sub)}</div>
      </div>
    `;
    makeCover($(".cover", el), r.id + track.id);
    el.addEventListener("click", () => {
      startPlayback([track.id, ...catalog.filter(t=>t.id!==track.id).map(t=>t.id)]);
      toast(`Playing: ${r.title}`);
    });
    return el;
  }

  function tile({title, desc, badge}, seed){
    const el = document.createElement("div");
    el.className = "tile";
    el.innerHTML = `
      <h4>${escapeHtml(title)}</h4>
      <p>${escapeHtml(desc)}</p>
      <span class="tile-badge">${escapeHtml(badge)}</span>
    `;
    // subtle tint
    el.style.background = `linear-gradient(135deg, rgba(255,255,255,.08), rgba(255,255,255,.04)),
      radial-gradient(900px 500px at 10% 20%, rgba(0,247,255,.10), transparent 55%),
      radial-gradient(900px 500px at 85% 65%, rgba(124,77,255,.10), transparent 55%)`;
    el.addEventListener("click", () => {
      const picks = pickByKeyword(title);
      if(picks.length){
        startPlayback(picks.map(p=>p.id));
        toast(`Starting: ${title}`);
        switchTab("listen");
      }else{
        toast(`${title}: no matches yet`);
      }
    });
    return el;
  }

  function rowTrack(t, idx){
    const el = document.createElement("div");
    el.className = "row";
    el.innerHTML = `
      <div class="row-left">
        <div class="cover sm" aria-hidden="true"></div>
        <div class="row-meta">
          <div class="row-title">${escapeHtml(t.title)}</div>
          <div class="row-sub">${escapeHtml(t.artist)} • ${escapeHtml(t.album)}</div>
        </div>
      </div>
      <div class="row-right">
        <span class="pillmini">${escapeHtml(t.heat)}</span>
        <span class="muted">${escapeHtml(t.dur)}</span>
      </div>
    `;
    makeCover($(".cover", el), t.id);
    el.addEventListener("click", () => {
      startPlayback(buildQueueFromTrack(t.id));
      toast(`Playing: ${t.title}`);
    });
    return el;
  }

  function qItem(tid){
    const t = catalog.find(x=>x.id===tid);
    if(!t) return null;
    const el = document.createElement("div");
    el.className = "qitem";
    el.innerHTML = `
      <div class="cover sm" aria-hidden="true"></div>
      <div class="qmeta">
        <div class="qtitle">${escapeHtml(t.title)}</div>
        <div class="qsub">${escapeHtml(t.artist)} • ${escapeHtml(t.album)}</div>
      </div>
    `;
    makeCover($(".cover", el), t.id + "_q");
    el.addEventListener("click", () => {
      const idx = state.queue.indexOf(tid);
      if(idx >= 0){
        state.index = idx;
        syncNowPlaying(true);
        toast(`Up next: ${t.title}`);
      }
    });
    return el;
  }

  function renderAll(){
    // Listen
    releaseRail.innerHTML = "";
    releases.forEach(r => releaseRail.appendChild(cardRelease(r)));

    mixGrid.innerHTML = "";
    mixes.forEach(m => {
      const el = document.createElement("div");
      el.className = "tile";
      el.innerHTML = `
        <h4>${escapeHtml(m.title)}</h4>
        <p>${escapeHtml(m.sub)}</p>
        <span class="tile-badge">${escapeHtml(m.badge)}</span>
      `;
      el.addEventListener("click", () => {
        startPlayback(m.seed);
        toast(`Mix: ${m.title}`);
      });
      mixGrid.appendChild(el);
    });

    topList.innerHTML = "";
    catalog.slice(0, 6).forEach((t,i) => topList.appendChild(rowTrack(t,i)));

    // Browse
    browseGrid.innerHTML = "";
    browseTiles.forEach((t,i) => browseGrid.appendChild(tile(t, "b"+i)));

    roomsGrid.innerHTML = "";
    rooms.forEach((r,i) => roomsGrid.appendChild(tile(r, "room"+i)));

    // Radio
    renderStations();

    // Library initial
    renderLibrary("playlists");
  }

  function renderStations(){
    stationGrid.innerHTML = "";
    shuffled(stations).forEach((s,i) => {
      const el = document.createElement("div");
      el.className = "tile";
      el.innerHTML = `
        <h4>${escapeHtml(s.title)}</h4>
        <p>${escapeHtml(s.desc)}</p>
        <span class="tile-badge">${escapeHtml(s.badge)}</span>
      `;
      el.addEventListener("click", () => {
        startPlayback(s.seed);
        toast(`Station: ${s.title}`);
      });
      stationGrid.appendChild(el);
    });
  }

  // ===== Navigation =====
  function switchTab(tab){
    tabBtns.forEach(b => b.classList.toggle("active", b.dataset.tab === tab));
    Object.entries(pages).forEach(([k, el]) => {
      el.classList.toggle("active", k === tab);
    });

    const active = pages[tab];
    if(active){
      pageTitle.textContent = active.dataset.title || "—";
      pageSub.textContent = active.dataset.sub || "";
    }

    // if user hits search tab, focus input
    if(tab === "search"){
      setTimeout(() => searchInput.focus(), 60);
    }
  }

  tabBtns.forEach(btn => {
    btn.addEventListener("click", () => {
      const tab = btn.dataset.tab;
      switchTab(tab);
    });
  });

  // Clicking the search bar should push to Search page (Apple-ish)
  searchInput.addEventListener("focus", () => {
    switchTab("search");
  });

  // ===== Library UI =====
  $$(".library-tabs .chip").forEach(ch => {
    ch.addEventListener("click", () => {
      $$(".library-tabs .chip").forEach(x => {
        x.classList.toggle("active", x === ch);
        x.setAttribute("aria-selected", x === ch ? "true" : "false");
      });
      renderLibrary(ch.dataset.lib);
    });
  });

  function renderLibrary(kind){
    const box = document.createElement("div");
    box.className = "list";

    if(kind === "playlists"){
      mixes.forEach(m => {
        const el = document.createElement("div");
        el.className = "row";
        el.innerHTML = `
          <div class="row-left">
            <div class="cover sm" aria-hidden="true"></div>
            <div class="row-meta">
              <div class="row-title">${escapeHtml(m.title)}</div>
              <div class="row-sub">${escapeHtml(m.sub)}</div>
            </div>
          </div>
          <div class="row-right">
            <span class="pillmini">Playlist</span>
            <span class="muted">${m.seed.length} tracks</span>
          </div>
        `;
        makeCover($(".cover", el), "pl_"+m.id);
        el.addEventListener("click", () => {
          startPlayback(m.seed);
          toast(`Playlist: ${m.title}`);
        });
        box.appendChild(el);
      });
    } else if(kind === "albums"){
      releases.forEach(r => {
        const el = document.createElement("div");
        el.className = "row";
        el.innerHTML = `
          <div class="row-left">
            <div class="cover sm" aria-hidden="true"></div>
            <div class="row-meta">
              <div class="row-title">${escapeHtml(r.title)}</div>
              <div class="row-sub">${escapeHtml(r.sub)}</div>
            </div>
          </div>
          <div class="row-right">
            <span class="pillmini">Album</span>
          </div>
        `;
        makeCover($(".cover", el), "al_"+r.id);
        el.addEventListener("click", () => {
          const t = catalog.find(x=>x.id===r.ref) || catalog[0];
          startPlayback(buildQueueFromTrack(t.id));
          toast(`Album: ${r.title}`);
        });
        box.appendChild(el);
      });
    } else if(kind === "songs"){
      catalog.forEach((t,i) => box.appendChild(rowTrack(t,i)));
    } else {
      const el = document.createElement("div");
      el.className = "tile";
      el.innerHTML = `
        <h4>Downloads</h4>
        <p>This is a UI mock. Hook this to real files later (HLS/MP3) and a backend library.</p>
        <span class="tile-badge">Offline-ready architecture</span>
      `;
      box.appendChild(el);
    }

    libraryPane.innerHTML = "";
    libraryPane.appendChild(box);
  }

  // ===== Search =====
  const SEARCH_DEBOUNCE = 120;
  let sTimer = null;

  searchInput.addEventListener("input", () => {
    clearTimeout(sTimer);
    sTimer = setTimeout(() => doSearch(searchInput.value.trim()), SEARCH_DEBOUNCE);
    clearSearch.style.display = searchInput.value.trim() ? "inline-block" : "none";
  });

  clearSearch.addEventListener("click", () => {
    searchInput.value = "";
    clearSearch.style.display = "none";
    doSearch("");
    searchInput.focus();
  });

  function doSearch(q){
    resultsList.innerHTML = "";
    if(!q){
      resultsMeta.textContent = "Start typing to search.";
      return;
    }

    const nq = q.toLowerCase();
    const found = catalog.filter(t =>
      t.title.toLowerCase().includes(nq) ||
      t.album.toLowerCase().includes(nq) ||
      t.mood.some(m => m.toLowerCase().includes(nq)) ||
      t.artist.toLowerCase().includes(nq)
    );

    resultsMeta.textContent = found.length ? `${found.length} result${found.length===1?"":"s"} for “${q}”` : `No results for “${q}”`;
    found.forEach((t,i) => {
      const el = rowTrack(t,i);
      resultsList.appendChild(el);
    });
  }

  // ===== Playback (simulated) =====
  const state = {
    queue: [],
    index: 0,
    isPlaying: false,
    shuffle: false,
    repeat: "off", // off | one | all
    // simulated timeline
    elapsed: 0,
    duration: 0,
    tick: null,
  };

  function parseDur(d){
    const [m,s] = d.split(":").map(Number);
    return (m*60)+s;
  }
  function fmtTime(sec){
    sec = Math.max(0, Math.floor(sec));
    const m = Math.floor(sec/60);
    const s = sec%60;
    return `${m}:${String(s).padStart(2,"0")}`;
  }

  function buildQueueFromTrack(trackId){
    // “Apple-ish”: play selected then follow with same album first, then rest
    const t = catalog.find(x=>x.id===trackId) || catalog[0];
    const sameAlbum = catalog.filter(x=>x.album===t.album && x.id!==t.id).map(x=>x.id);
    const rest = catalog.filter(x=>x.album!==t.album && x.id!==t.id).map(x=>x.id);
    return [t.id, ...sameAlbum, ...rest];
  }

  function startPlayback(queueIds){
    if(!Array.isArray(queueIds) || !queueIds.length){
      toast("Nothing to play.");
      return;
    }
    state.queue = queueIds.slice();
    state.index = 0;
    state.elapsed = 0;
    syncNowPlaying(true);
    setPlaying(true);
    renderQueue();
    renderVibe();
  }

  function currentTrack(){
    const id = state.queue[state.index];
    return catalog.find(x=>x.id===id) || null;
  }

  function syncNowPlaying(resetTime=false){
    const t = currentTrack();
    if(!t){
      miniTitle.textContent = "Not playing";
      miniArtist.textContent = "—";
      playerTitle.textContent = "—";
      playerArtist.textContent = "—";
      miniBar.style.width = "0%";
      playerCover && makeCover(playerCover, "none");
      miniCover && makeCover(miniCover, "none");
      return;
    }

    miniTitle.textContent = t.title;
    miniArtist.textContent = `${t.artist} • ${t.album}`;
    playerTitle.textContent = t.title;
    playerArtist.textContent = `${t.artist} • ${t.album}`;

    makeCover(miniCover, t.id + "_mini");
    makeCover(playerCover, t.id + "_player");

    state.duration = parseDur(t.dur);
    if(resetTime) state.elapsed = 0;

    tDur.textContent = fmtTime(state.duration);
    tCur.textContent = fmtTime(state.elapsed);
    seek.value = Math.round((state.elapsed / state.duration) * 1000) || 0;

    const pct = state.duration ? (state.elapsed / state.duration) * 100 : 0;
    miniBar.style.width = `${pct}%`;

    updatePlayIcons();
  }

  function setPlaying(on){
    state.isPlaying = !!on;
    updatePlayIcons();

    if(state.tick) clearInterval(state.tick);
    if(state.isPlaying){
      state.tick = setInterval(() => {
        state.elapsed += 0.25; // 4 fps simulation
        if(state.elapsed >= state.duration){
          handleTrackEnd();
        } else {
          syncNowPlaying(false);
        }
      }, 250);
    }
  }

  function updatePlayIcons(){
    // mini icon
    if(state.isPlaying){
      playIcon.setAttribute("viewBox","0 0 24 24");
      playIcon.innerHTML = `<path d="M7 6h4v12H7zM13 6h4v12h-4z"/>`;
      playerPlay.textContent = "❚❚";
    }else{
      playIcon.setAttribute("viewBox","0 0 24 24");
      playIcon.innerHTML = `<path d="M8 5v14l12-7-12-7Z"/>`;
      playerPlay.textContent = "▶";
    }
  }

  function handleTrackEnd(){
    if(state.repeat === "one"){
      state.elapsed = 0;
      syncNowPlaying(true);
      return;
    }
    if(state.index < state.queue.length - 1){
      state.index += 1;
      state.elapsed = 0;
      syncNowPlaying(true);
      return;
    }
    // end of queue
    if(state.repeat === "all"){
      state.index = 0;
      state.elapsed = 0;
      syncNowPlaying(true);
      return;
    }
    // stop
    setPlaying(false);
    state.elapsed = state.duration;
    syncNowPlaying(false);
  }

  function next(){
    if(!state.queue.length) return;
    if(state.shuffle){
      state.index = randInt(0, state.queue.length-1);
    } else if(state.index < state.queue.length - 1){
      state.index++;
    } else if(state.repeat === "all"){
      state.index = 0;
    }
    state.elapsed = 0;
    syncNowPlaying(true);
    renderQueue();
  }

  function prev(){
    if(!state.queue.length) return;
    if(state.elapsed > 2){
      state.elapsed = 0;
      syncNowPlaying(false);
      return;
    }
    if(state.shuffle){
      state.index = randInt(0, state.queue.length-1);
    } else if(state.index > 0){
      state.index--;
    } else if(state.repeat === "all"){
      state.index = state.queue.length-1;
    }
    state.elapsed = 0;
    syncNowPlaying(true);
    renderQueue();
  }

  // Controls
  playBtn.addEventListener("click", () => {
    if(!state.queue.length){
      startPlayback([catalog[0].id, ...catalog.slice(1).map(t=>t.id)]);
      return;
    }
    setPlaying(!state.isPlaying);
    toast(state.isPlaying ? "Play" : "Pause");
  });
  nextBtn.addEventListener("click", () => { next(); toast("Next"); });
  prevBtn.addEventListener("click", () => { prev(); toast("Previous"); });

  playerPlay.addEventListener("click", () => {
    if(!state.queue.length){
      startPlayback([catalog[0].id, ...catalog.slice(1).map(t=>t.id)]);
      return;
    }
    setPlaying(!state.isPlaying);
  });
  playerNext.addEventListener("click", next);
  playerPrev.addEventListener("click", prev);

  shuffleBtn.addEventListener("click", () => {
    state.shuffle = !state.shuffle;
    shuffleBtn.style.borderColor = state.shuffle ? "rgba(0,247,255,.38)" : "";
    toast(state.shuffle ? "Shuffle: On" : "Shuffle: Off");
  });

  repeatBtn.addEventListener("click", () => {
    state.repeat = state.repeat === "off" ? "all" : state.repeat === "all" ? "one" : "off";
    repeatBtn.style.borderColor = state.repeat !== "off" ? "rgba(124,77,255,.38)" : "";
    toast(`Repeat: ${state.repeat}`);
  });

  seek.addEventListener("input", () => {
    if(!state.duration) return;
    const pct = Number(seek.value) / 1000;
    state.elapsed = pct * state.duration;
    syncNowPlaying(false);
  });

  vol.addEventListener("input", () => {
    // UI mock: just toast on release-ish
    // (Real audio: map to audio.volume)
  });

  // ===== Overlay open/close =====
  function openOverlay(){
    overlay.classList.add("show");
    overlay.setAttribute("aria-hidden","false");
    // trap-ish: focus close
    setTimeout(() => closePlayer.focus(), 40);
  }
  function closeOverlay(){
    overlay.classList.remove("show");
    overlay.setAttribute("aria-hidden","true");
  }

  openPlayer.addEventListener("click", openOverlay);
  closePlayer.addEventListener("click", closeOverlay);
  overlay.addEventListener("click", (e) => {
    if(e.target === overlay) closeOverlay();
  });
  document.addEventListener("keydown", (e) => {
    if(e.key === "Escape" && overlay.classList.contains("show")) closeOverlay();
  });

  // ===== Queue / vibe =====
  function renderQueue(){
    queueList.innerHTML = "";
    if(!state.queue.length){
      queueList.innerHTML = `<div class="muted small">Queue is empty.</div>`;
      return;
    }
    state.queue.forEach((tid, i) => {
      const el = qItem(tid);
      if(!el) return;
      if(i === state.index){
        el.style.borderColor = "rgba(0,247,255,.38)";
        el.style.background = "linear-gradient(135deg, rgba(0,247,255,.10), rgba(124,77,255,.08))";
      }
      queueList.appendChild(el);
    });
  }

  clearQueue.addEventListener("click", () => {
    state.queue = [];
    state.index = 0;
    state.elapsed = 0;
    setPlaying(false);
    syncNowPlaying(true);
    renderQueue();
    toast("Queue cleared");
  });

  function renderVibe(){
    vibeTags.innerHTML = "";
    const t = currentTrack();
    const tags = t ? [...t.mood] : ["Cosmic","Underground","Clean Mix","Late Night"];
    const extras = [
      "Word Weight", "No Clout Chase", "Night Drive", "Headphones", "Cold Precision",
      "Neon", "Quiet Pressure", "Signal", "Craft", "Grit"
    ];
    const chosen = [...tags, ...shuffled(extras).slice(0, 5-tags.length)];
    chosen.forEach(txt => {
      const el = document.createElement("span");
      el.className = "tag";
      el.textContent = txt;
      vibeTags.appendChild(el);
    });
  }
  regenVibe.addEventListener("click", () => {
    renderVibe();
    toast("Vibe regenerated");
  });

  // ===== CTA actions =====
  playFeatured.addEventListener("click", () => {
    startPlayback(buildQueueFromTrack("t1"));
  });
  shuffleAll.addEventListener("click", () => {
    const ids = shuffled(catalog.map(t=>t.id));
    startPlayback(ids);
    state.shuffle = true;
    shuffleBtn.style.borderColor = "rgba(0,247,255,.38)";
    toast("Shuffle: All tracks");
  });

  startRadio.addEventListener("click", () => {
    const s = stations[0];
    startPlayback(s.seed);
    toast("Dei Radio: Started");
  });
  radioInfo.addEventListener("click", () => {
    toast("Radio is a UI mock—hook to a stream URL when ready.");
  });

  // Quick actions in headings
  document.addEventListener("click", (e) => {
    const btn = e.target.closest("[data-action]");
    if(!btn) return;
    const action = btn.dataset.action;
    if(action === "refresh"){
      toast("Refreshed rooms");
      roomsGrid.innerHTML = "";
      shuffled(rooms).forEach((r,i) => roomsGrid.appendChild(tile(r, "roomR"+i)));
    }
    if(action === "refresh-stations"){
      renderStations();
      toast("Stations shuffled");
    }
    if(action === "see-all"){
      const target = btn.dataset.target || "";
      if(target === "top") toast("Tip: Click any track to build a queue.");
      else toast("See All (hook to route later)");
    }
  });

  // ===== Helpers =====
  function pickByKeyword(word){
    const w = word.toLowerCase();
    return catalog.filter(t =>
      t.title.toLowerCase().includes(w) ||
      t.album.toLowerCase().includes(w) ||
      t.mood.some(m => m.toLowerCase().includes(w))
    );
  }

  function shuffled(arr){
    const a = arr.slice();
    for(let i=a.length-1;i>0;i--){
      const j = Math.floor(Math.random()*(i+1));
      [a[i],a[j]]=[a[j],a[i]];
    }
    return a;
  }
  function randInt(min,max){
    return Math.floor(Math.random()*(max-min+1))+min;
  }
  function escapeHtml(s){
    return String(s).replace(/[&<>"']/g, c => ({
      "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"
    }[c]));
  }

  // ===== Toast =====
  const toastEl = $("#toast");
  let toastTimer = null;
  function toast(msg){
    toastEl.textContent = msg;
    toastEl.classList.add("show");
    clearTimeout(toastTimer);
    toastTimer = setTimeout(() => toastEl.classList.remove("show"), 1300);
  }

  // ===== Stars canvas =====
  const canvas = $("#stars");
  const ctx = canvas.getContext("2d", { alpha: true });
  let W=0,H=0, stars=[];
  function resize(){
    const dpr = Math.min(2, window.devicePixelRatio || 1);
    W = canvas.width = Math.floor(window.innerWidth * dpr);
    H = canvas.height = Math.floor(window.innerHeight * dpr);
    canvas.style.width = window.innerWidth + "px";
    canvas.style.height = window.innerHeight + "px";
    makeStars(140);
    draw();
  }
  function makeStars(n){
    stars = [];
    for(let i=0;i<n;i++){
      stars.push({
        x: Math.random()*W,
        y: Math.random()*H,
        r: (Math.random()*1.4 + .4),
        a: Math.random()*0.55 + 0.15,
        tw: Math.random()*0.015 + 0.004,
      });
    }
  }
  function draw(){
    ctx.clearRect(0,0,W,H);
    ctx.fillStyle = "rgba(255,255,255,0.9)";
    for(const s of stars){
      s.a += (Math.random() - 0.5) * s.tw;
      s.a = Math.max(0.12, Math.min(0.9, s.a));
      ctx.globalAlpha = s.a;
      ctx.beginPath();
      ctx.arc(s.x, s.y, s.r, 0, Math.PI*2);
      ctx.fill();
    }
    ctx.globalAlpha = 1;
    requestAnimationFrame(draw);
  }
  window.addEventListener("resize", resize);

  // ===== Init =====
  renderAll();
  switchTab("listen");
  // set a default queue so the UI feels alive
  startPlayback(buildQueueFromTrack("t1"));
  setPlaying(false);

  resize();

  // update clears
  searchInput.addEventListener("blur", () => {
    // keep clear btn if text exists
    clearSearch.style.display = searchInput.value.trim() ? "inline-block" : "none";
  });

  // Mini player tap to open overlay (already wired)
})();
