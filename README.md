[index.html.html](https://github.com/user-attachments/files/28298787/index.html.html)
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RAW → JPEG · Adobe RGB Converter</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,500;1,9..144,300&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --bg: #0e0e0f; --surface: #161618; --surface2: #1e1e21;
    --border: rgba(255,255,255,0.08); --border-hover: rgba(255,255,255,0.16);
    --text: #f0ede8; --muted: #7a7870; --accent: #c8a96e; --accent2: #8bb4a0;
    --danger: #c86e6e; --success: #6ec88a;
    --mono: 'DM Mono', monospace; --serif: 'Fraunces', serif;
  }
  body { background: var(--bg); color: var(--text); font-family: var(--mono); min-height: 100vh; overflow-x: hidden; }
  body::before {
    content: ''; position: fixed; inset: 0; pointer-events: none; z-index: 0;
    background-image: linear-gradient(rgba(200,169,110,0.03) 1px, transparent 1px), linear-gradient(90deg, rgba(200,169,110,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
  }
  .container { position: relative; z-index: 1; max-width: 860px; margin: 0 auto; padding: 60px 24px 80px; }
  header { margin-bottom: 56px; }
  .eyebrow { font-size: 10px; font-weight: 500; letter-spacing: 0.2em; text-transform: uppercase; color: var(--accent); margin-bottom: 16px; display: flex; align-items: center; gap: 8px; }
  .eyebrow::before { content: ''; display: inline-block; width: 20px; height: 1px; background: var(--accent); }
  h1 { font-family: var(--serif); font-size: clamp(36px, 6vw, 58px); font-weight: 300; line-height: 1.1; letter-spacing: -0.02em; }
  h1 em { font-style: italic; color: var(--accent); }
  .subtitle { margin-top: 14px; font-size: 13px; color: var(--muted); line-height: 1.7; max-width: 480px; }
  .drop-zone {
    border: 1px dashed var(--border-hover); border-radius: 12px; padding: 56px 32px;
    text-align: center; cursor: pointer; transition: all 0.2s; background: var(--surface);
    position: relative; overflow: hidden; margin-bottom: 24px;
  }
  .drop-zone::before { content: ''; position: absolute; inset: 0; background: radial-gradient(ellipse at 50% 100%, rgba(200,169,110,0.05) 0%, transparent 70%); pointer-events: none; }
  .drop-zone:hover, .drop-zone.drag-over { border-color: var(--accent); background: #1a1a1c; }
  .drop-zone.drag-over { border-style: solid; }
  .drop-icon { width: 48px; height: 48px; margin: 0 auto 16px; color: var(--muted); opacity: 0.6; transition: all 0.2s; }
  .drop-zone:hover .drop-icon, .drop-zone.drag-over .drop-icon { color: var(--accent); opacity: 1; }
  .drop-title { font-family: var(--serif); font-size: 18px; font-weight: 300; color: var(--text); margin-bottom: 8px; }
  .drop-hint { font-size: 12px; color: var(--muted); margin-bottom: 20px; }
  .formats { display: flex; justify-content: center; gap: 6px; flex-wrap: wrap; }
  .fmt-badge { font-size: 10px; font-weight: 500; letter-spacing: 0.1em; padding: 3px 8px; border-radius: 3px; border: 1px solid var(--border); color: var(--muted); }
  .browse-btn { display: inline-flex; align-items: center; gap: 6px; margin-top: 20px; padding: 9px 20px; border: 1px solid var(--border-hover); border-radius: 6px; background: transparent; color: var(--text); font-family: var(--mono); font-size: 12px; cursor: pointer; transition: all 0.15s; }
  .browse-btn:hover { border-color: var(--accent); color: var(--accent); }
  input[type="file"] { display: none; }
  .settings-panel { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 20px 24px; margin-bottom: 24px; display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; }
  @media (max-width: 600px) { .settings-panel { grid-template-columns: 1fr; } }
  .setting-group label { display: block; font-size: 10px; font-weight: 500; letter-spacing: 0.12em; text-transform: uppercase; color: var(--muted); margin-bottom: 8px; }
  select { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-family: var(--mono); font-size: 12px; padding: 8px 28px 8px 10px; cursor: pointer; outline: none; appearance: none; width: 100%; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%237a7870'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 10px center; }
  select:focus { border-color: var(--accent); }
  input[type="range"] { -webkit-appearance: none; width: 100%; height: 3px; background: var(--border-hover); border-radius: 2px; outline: none; margin-top: 10px; }
  input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; width: 14px; height: 14px; border-radius: 50%; background: var(--accent); cursor: pointer; }
  .range-value { font-size: 11px; color: var(--accent); margin-top: 6px; display: block; }
  .queue { display: flex; flex-direction: column; gap: 8px; margin-bottom: 24px; }
  .queue-item { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 14px 18px; display: flex; align-items: center; gap: 14px; transition: border-color 0.2s; }
  .queue-item.done { border-color: rgba(110,200,138,0.25); }
  .queue-item.error { border-color: rgba(200,110,110,0.25); }
  .queue-item.processing { border-color: rgba(200,169,110,0.3); }
  .file-icon { width: 36px; height: 36px; border-radius: 6px; background: var(--surface2); display: flex; align-items: center; justify-content: center; font-size: 10px; font-weight: 500; letter-spacing: 0.05em; color: var(--muted); flex-shrink: 0; border: 1px solid var(--border); overflow: hidden; }
  .file-icon img { width: 100%; height: 100%; object-fit: cover; border-radius: 5px; }
  .file-info { flex: 1; min-width: 0; }
  .file-name { font-size: 13px; color: var(--text); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .file-meta { font-size: 11px; color: var(--muted); margin-top: 2px; display: flex; gap: 10px; }
  .file-status { flex-shrink: 0; font-size: 11px; font-weight: 500; display: flex; align-items: center; gap: 6px; }
  .status-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--muted); }
  .queue-item.processing .status-dot { background: var(--accent); animation: pulse 1s infinite; }
  .queue-item.done .status-dot { background: var(--success); }
  .queue-item.error .status-dot { background: var(--danger); }
  @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.3; } }
  .progress-bar { height: 2px; background: var(--border); border-radius: 1px; margin-top: 8px; overflow: hidden; }
  .progress-fill { height: 100%; background: var(--accent); border-radius: 1px; transition: width 0.3s ease; width: 0%; }
  .download-btn { flex-shrink: 0; padding: 6px 12px; border: 1px solid rgba(110,200,138,0.4); border-radius: 5px; background: transparent; color: var(--success); font-family: var(--mono); font-size: 11px; cursor: pointer; transition: all 0.15s; text-decoration: none; display: inline-flex; align-items: center; gap: 5px; }
  .download-btn:hover { background: rgba(110,200,138,0.08); border-color: var(--success); }
  .remove-btn { flex-shrink: 0; width: 28px; height: 28px; border-radius: 5px; border: 1px solid var(--border); background: transparent; color: var(--muted); cursor: pointer; font-size: 14px; display: flex; align-items: center; justify-content: center; transition: all 0.15s; }
  .remove-btn:hover { border-color: var(--danger); color: var(--danger); }
  .action-bar { display: flex; gap: 12px; align-items: center; }
  .convert-btn { flex: 1; padding: 14px 24px; border-radius: 8px; background: var(--accent); color: #0e0e0f; border: none; font-family: var(--mono); font-size: 13px; font-weight: 500; cursor: pointer; transition: all 0.2s; letter-spacing: 0.05em; }
  .convert-btn:hover { background: #d4b87a; }
  .convert-btn:disabled { opacity: 0.4; cursor: not-allowed; }
  .download-all-btn { padding: 14px 20px; border-radius: 8px; background: transparent; color: var(--text); border: 1px solid var(--border-hover); font-family: var(--mono); font-size: 13px; cursor: pointer; transition: all 0.15s; white-space: nowrap; }
  .download-all-btn:hover { border-color: var(--accent2); color: var(--accent2); }
  .download-all-btn:disabled { opacity: 0.3; cursor: not-allowed; }
  .cs-notice { background: var(--surface); border: 1px solid var(--border); border-left: 3px solid var(--accent); border-radius: 8px; padding: 14px 18px; margin-top: 24px; display: flex; gap: 12px; align-items: flex-start; }
  .cs-notice svg { flex-shrink: 0; margin-top: 1px; color: var(--accent); }
  .cs-notice-text { font-size: 12px; line-height: 1.7; color: var(--muted); }
  .cs-notice-text strong { color: var(--text); font-weight: 500; }
  footer { margin-top: 64px; padding-top: 24px; border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; }
  footer span { opacity: 0.6; }
  .tag { display: inline-flex; align-items: center; gap: 4px; font-size: 10px; padding: 3px 8px; border-radius: 3px; border: 1px solid var(--border); color: var(--muted); }
  .tag.active { border-color: rgba(139,180,160,0.4); color: var(--accent2); }
  canvas { display: none; }
  .error-detail { font-size: 10px; color: var(--danger); opacity: 0.8; margin-top: 2px; }
</style>
</head>
<body>
<div class="container">
  <header>
    <div class="eyebrow">RAW Processor</div>
    <h1>Canon RAW<br>to <em>JPEG</em></h1>
    <p class="subtitle">Convert .CR2 and .CR3 files to JPEG with Adobe RGB (1998) color space encoding — fully in your browser, nothing uploaded.</p>
  </header>

  <div class="drop-zone" id="dropZone">
    <svg class="drop-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
      <path d="M4 16l4-4m0 0l4 4m-4-4v9M20 16l-4-4m0 0l-4 4m4-4V3"/>
      <rect x="3" y="3" width="18" height="18" rx="3" stroke-dasharray="3 2"/>
    </svg>
    <p class="drop-title">Drop Canon RAW files here</p>
    <p class="drop-hint">or click to browse your files</p>
    <div class="formats">
      <span class="fmt-badge">CR3</span><span class="fmt-badge">CR2</span><span class="fmt-badge">CRW</span>
      <span class="fmt-badge">NEF</span><span class="fmt-badge">ARW</span><span class="fmt-badge">RAF</span>
      <span class="fmt-badge">RW2</span><span class="fmt-badge">DNG</span>
    </div>
    <button class="browse-btn" onclick="fileInput.click()">
      <svg width="12" height="12" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.5">
        <rect x="2" y="4" width="12" height="10" rx="1.5"/><path d="M5 4V3a1 1 0 011-1h4a1 1 0 011 1v1"/>
      </svg>
      Browse Files
    </button>
    <input type="file" id="fileInput" accept=".cr2,.cr3,.crw,.raf,.nef,.arw,.rw2,.dng" multiple>
  </div>

  <div class="settings-panel">
    <div class="setting-group">
      <label>Color Space</label>
      <select id="colorSpace">
        <option value="adobe-rgb" selected>Adobe RGB (1998)</option>
        <option value="srgb">sRGB</option>
        <option value="prophoto">ProPhoto RGB</option>
      </select>
    </div>
    <div class="setting-group">
      <label>JPEG Quality</label>
      <input type="range" id="quality" min="70" max="100" value="95" step="1">
      <span class="range-value" id="qualityVal">95%</span>
    </div>
    <div class="setting-group">
      <label>Output Size</label>
      <select id="outputSize">
        <option value="original" selected>Original (Full Res)</option>
        <option value="2048">Long edge 2048px</option>
        <option value="1920">Long edge 1920px</option>
        <option value="1280">Long edge 1280px</option>
      </select>
    </div>
  </div>

  <div class="queue" id="queue">
    <div id="emptyState" style="display:flex;justify-content:center;align-items:center;padding:16px 0;gap:8px;font-size:12px;color:var(--muted)">
      <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="8" cy="8" r="6"/><path d="M8 5v3l2 2"/></svg>
      No files queued yet
    </div>
  </div>

  <div class="action-bar">
    <button class="convert-btn" id="convertBtn" onclick="convertAll()" disabled>Convert to JPEG</button>
    <button class="download-all-btn" id="downloadAllBtn" onclick="downloadAll()" disabled>Download All</button>
  </div>

  <div class="cs-notice">
    <svg width="16" height="16" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.5">
      <circle cx="8" cy="8" r="6"/><path d="M8 7v4M8 5.5v.5"/>
    </svg>
    <div class="cs-notice-text">
      <strong>Adobe RGB (1998)</strong> is embedded in the output JPEG via ICC profile. Reads the full-resolution embedded JPEG preview from the RAW container (CR3: ISOBMFF/mdat scan; CR2: TIFF IFD walk). Colors display correctly in <strong>Lightroom</strong>, <strong>Photoshop</strong>, and <strong>Preview (macOS)</strong>.
    </div>
  </div>

  <footer>
    <span>All processing is local · No files leave your device</span>
    <div style="display:flex;gap:6px;">
      <span class="tag active">Adobe RGB</span>
      <span class="tag">ICC Profile</span>
      <span class="tag">CR3 / CR2</span>
    </div>
  </footer>
</div>

<canvas id="workCanvas"></canvas>

<script>
// ─── State ────────────────────────────────────────────────────────────────────
const files = new Map();
let counter = 0;

// ─── CR3 / ISOBMFF parser ─────────────────────────────────────────────────────
// CR3 is wrapped in an ISO Base Media File Format container (like MP4).
// Structure: ftyp box → moov box (with Canon CMT* sub-boxes) → mdat box(es)
// The largest mdat typically contains the full-res JPEG preview.
// We scan all boxes at the top level, collect all mdat regions, then
// scan each for JPEG SOI/EOI markers.

function parseCR3(buffer) {
  const view = new DataView(buffer);
  const bytes = new Uint8Array(buffer);
  const len = buffer.byteLength;

  // Verify ftyp starts with 'crx ' or 'CR3 '
  // Box layout: [4 bytes size][4 bytes type][...data]
  // Extended size: size==1 means next 8 bytes are uint64 size

  function readBox(offset) {
    if (offset + 8 > len) return null;
    let size = view.getUint32(offset);
    const typeBytes = bytes.slice(offset + 4, offset + 8);
    const type = String.fromCharCode(...typeBytes);
    let headerSize = 8;
    if (size === 1) {
      // 64-bit extended size
      const hi = view.getUint32(offset + 8);
      const lo = view.getUint32(offset + 12);
      size = hi * 0x100000000 + lo;
      headerSize = 16;
    }
    if (size === 0) size = len - offset; // box extends to EOF
    return { type, offset, size, headerSize, dataOffset: offset + headerSize, dataSize: size - headerSize };
  }

  // Walk top-level boxes
  const mdatRegions = [];
  let offset = 0;
  let isCR3 = false;

  while (offset < len) {
    const box = readBox(offset);
    if (!box || box.size < 8) break;

    if (box.type === 'ftyp') {
      // Check brand
      const brand = String.fromCharCode(...bytes.slice(box.dataOffset, box.dataOffset + 4));
      if (brand === 'crx ' || brand === 'CR3 ') isCR3 = true;
    }

    if (box.type === 'mdat') {
      mdatRegions.push({ start: box.dataOffset, size: box.dataSize });
    }

    // Also walk moov → look for uuid boxes Canon uses (CMT1 etc stored as uuid)
    if (box.type === 'moov') {
      // Scan moov children for uuid boxes that might contain JPEG
      let inner = box.dataOffset;
      while (inner < box.offset + box.size) {
        const child = readBox(inner);
        if (!child || child.size < 8) break;
        // Canon stores a JPEG preview in a uuid box with specific GUID
        // GUID: 85c0b687820f11e08111f4ce462b6a48
        if (child.type === 'uuid') {
          const guid = Array.from(bytes.slice(child.dataOffset, child.dataOffset + 16))
            .map(b => b.toString(16).padStart(2,'0')).join('');
          // Canon preview GUID
          if (guid === '85c0b687820f11e08111f4ce462b6a48') {
            // JPEG starts after the UUID header + GUID (16 bytes)
            const jStart = child.dataOffset + 16;
            if (bytes[jStart] === 0xFF && bytes[jStart+1] === 0xD8) {
              const jEnd = findJpegEnd(bytes, jStart);
              if (jEnd > jStart) {
                return new Blob([bytes.slice(jStart, jEnd + 2)], { type: 'image/jpeg' });
              }
            }
          }
        }
        inner += child.size;
      }
    }

    offset += box.size;
  }

  // Scan mdat regions for JPEG markers — largest valid JPEG wins
  let bestBlob = null;
  let bestSize = 0;

  for (const region of mdatRegions) {
    const end = Math.min(region.start + region.size, len);
    // Scan for JPEG SOI
    for (let i = region.start; i < end - 1; i++) {
      if (bytes[i] === 0xFF && bytes[i+1] === 0xD8) {
        const jEnd = findJpegEnd(bytes, i);
        if (jEnd > i) {
          const jLen = jEnd - i + 2;
          if (jLen > bestSize && jLen > 50000) { // at least 50KB — skip tiny thumbs
            bestSize = jLen;
            bestBlob = new Blob([bytes.slice(i, jEnd + 2)], { type: 'image/jpeg' });
          }
          i = jEnd + 1; // skip past this JPEG
        }
      }
    }
  }

  return bestBlob;
}

function findJpegEnd(bytes, startOffset) {
  // Scan for EOI marker FF D9
  for (let i = startOffset + 2; i < bytes.length - 1; i++) {
    if (bytes[i] === 0xFF && bytes[i+1] === 0xD9) return i;
  }
  return -1;
}

// ─── TIFF/CR2 IFD walker ──────────────────────────────────────────────────────
function extractFromTIFF(buffer) {
  const view = new DataView(buffer);
  const bytes = new Uint8Array(buffer);
  const le = view.getUint16(0) === 0x4949;
  const r16 = o => view.getUint16(o, le);
  const r32 = o => view.getUint32(o, le);

  let bestBlob = null;
  let bestSize = 0;

  function scanIFD(ifdOffset) {
    if (ifdOffset === 0 || ifdOffset + 2 > buffer.byteLength) return;
    try {
      const n = r16(ifdOffset);
      if (n > 512) return; // sanity
      let jpegOff = 0, jpegLen = 0, subIFDOff = 0, exifIFDOff = 0;

      for (let i = 0; i < n; i++) {
        const e = ifdOffset + 2 + i * 12;
        if (e + 12 > buffer.byteLength) break;
        const tag = r16(e);
        const val = r32(e + 8);
        if (tag === 0x0201) jpegOff = val;
        if (tag === 0x0202) jpegLen = val;
        if (tag === 0x014A) subIFDOff = val;  // SubIFDs
        if (tag === 0x8769) exifIFDOff = val; // Exif IFD
      }

      if (jpegOff && jpegLen && jpegOff + jpegLen <= buffer.byteLength) {
        if (bytes[jpegOff] === 0xFF && bytes[jpegOff+1] === 0xD8) {
          if (jpegLen > bestSize) {
            bestSize = jpegLen;
            bestBlob = new Blob([bytes.slice(jpegOff, jpegOff + jpegLen)], { type: 'image/jpeg' });
          }
        }
      }

      // Walk next IFD
      const nextOff = r32(ifdOffset + 2 + n * 12);
      if (nextOff && nextOff !== ifdOffset) scanIFD(nextOff);
      if (subIFDOff) scanIFD(subIFDOff);
      if (exifIFDOff) scanIFD(exifIFDOff);
    } catch(e) {}
  }

  const ifd0 = r32(4);
  scanIFD(ifd0);
  return bestBlob;
}

// ─── Brute-force JPEG scan (last resort) ─────────────────────────────────────
function bruteForceJpeg(buffer) {
  const bytes = new Uint8Array(buffer);
  let bestBlob = null, bestSize = 0;

  for (let i = 0; i < bytes.length - 1; i++) {
    if (bytes[i] === 0xFF && bytes[i+1] === 0xD8) {
      const end = findJpegEnd(bytes, i);
      if (end > i) {
        const sz = end - i + 2;
        if (sz > bestSize && sz > 50000) {
          bestSize = sz;
          bestBlob = new Blob([bytes.slice(i, end + 2)], { type: 'image/jpeg' });
        }
        i = end + 1;
      }
    }
  }
  return bestBlob;
}

// ─── Main extraction dispatcher ───────────────────────────────────────────────
async function extractJpegFromRAW(buffer) {
  const bytes = new Uint8Array(buffer);
  const view = new DataView(buffer);

  // Already a JPEG?
  if (bytes[0] === 0xFF && bytes[1] === 0xD8) {
    return new Blob([buffer], { type: 'image/jpeg' });
  }

  // CR3 / ISOBMFF: check for 'ftyp' box at offset 4 (after 4-byte size)
  const maybeType = String.fromCharCode(bytes[4], bytes[5], bytes[6], bytes[7]);
  if (maybeType === 'ftyp') {
    const blob = parseCR3(buffer);
    if (blob) return blob;
    // Fall through to brute force if CR3 parse failed
    return bruteForceJpeg(buffer);
  }

  // CR2 / TIFF: little-endian 0x4949 or big-endian 0x4D4D
  const magic = view.getUint16(0);
  if (magic === 0x4949 || magic === 0x4D4D) {
    const blob = extractFromTIFF(buffer);
    if (blob) return blob;
  }

  // Last resort: brute-force scan
  return bruteForceJpeg(buffer);
}

// ─── Adobe RGB (1998) ICC Profile ────────────────────────────────────────────
// Minimal valid ICC v2 profile for Adobe RGB 1998
// Built from spec: header + tag table + rXYZ/gXYZ/bXYZ + wtpt + rTRC/gTRC/bTRC
function buildAdobeRGBICC() {
  function u32(n) { return [(n>>>24)&0xFF,(n>>>16)&0xFF,(n>>>8)&0xFF,n&0xFF]; }
  function u16(n) { return [(n>>>8)&0xFF,n&0xFF]; }
  function tag(s) { return [...s].map(c=>c.charCodeAt(0)); }
  function s15f16(n) { const v=Math.round(n*65536); return u32(v>>>0); }
  function XYZtype(X,Y,Z) {
    return [...tag('XYZ '), 0,0,0,0, ...s15f16(X), ...s15f16(Y), ...s15f16(Z)];
  }
  // Gamma curve: type=0x63757276, reserved, count=1, value as u16 s7.8 fixed
  // Adobe RGB gamma = 563/256 ≈ 2.19921875
  function gammaTag() {
    return [...tag('curv'), 0,0,0,0, ...u32(1), ...u16(Math.round(2.19921875*256))];
  }
  // Adobe RGB (1998) primaries and white point (D65) in XYZ
  // rXYZ: 0.60974, 0.31111, 0.01947
  // gXYZ: 0.20528, 0.62567, 0.06087
  // bXYZ: 0.14919, 0.06322, 0.74457
  // wtpt: 0.95045, 1.00000, 1.08905 (D65)
  const rXYZ = XYZtype(0.60974, 0.31111, 0.01947);
  const gXYZ = XYZtype(0.20528, 0.62567, 0.06087);
  const bXYZ = XYZtype(0.14919, 0.06322, 0.74457);
  const wtpt = XYZtype(0.95045, 1.00000, 1.08905);
  const gTag = gammaTag();

  // Tag table: 9 tags
  // Tags: desc, cprt, wtpt, rXYZ, gXYZ, bXYZ, rTRC, gTRC, bTRC
  // desc (mluc or desc) - use simple 'desc' type for v2
  const descStr = 'Adobe RGB (1998)';
  const descData = [
    ...tag('desc'), 0,0,0,0,
    ...u32(descStr.length+1),
    ...[...descStr].map(c=>c.charCodeAt(0)), 0,
    0,0,0,0,0,0,0,0, // localized desc (empty)
    0,0,0,0,
    0,0,0,0,0,0,0,0,
  ];
  const cprtStr = 'Copyright 1999 Adobe Systems';
  const cprtData = [...tag('text'), 0,0,0,0, ...[...cprtStr].map(c=>c.charCodeAt(0)), 0];

  // Build tag offsets. Header=128 bytes, tag count u32, then 9×12-byte records
  const numTags = 9;
  const tagTableSize = 4 + numTags * 12;
  const headerSize = 128;
  let cursor = headerSize + tagTableSize;

  function align4(arr) {
    while (arr.length % 4 !== 0) arr.push(0);
    return arr;
  }

  const tags = [
    { sig: 'desc', data: align4([...descData]) },
    { sig: 'cprt', data: align4([...cprtData]) },
    { sig: 'wtpt', data: [...wtpt] },
    { sig: 'rXYZ', data: [...rXYZ] },
    { sig: 'gXYZ', data: [...gXYZ] },
    { sig: 'bXYZ', data: [...bXYZ] },
    { sig: 'rTRC', data: [...gTag] },
    { sig: 'gTRC', data: [...gTag] },
    { sig: 'bTRC', data: [...gTag] },
  ];

  const offsets = [];
  for (const t of tags) { offsets.push(cursor); cursor += t.data.length; }
  const totalSize = cursor;

  // Header (128 bytes)
  const header = new Array(128).fill(0);
  const setU32 = (arr,o,v) => { arr[o]=(v>>>24)&0xFF; arr[o+1]=(v>>>16)&0xFF; arr[o+2]=(v>>>8)&0xFF; arr[o+3]=v&0xFF; };
  setU32(header, 0, totalSize);
  // version 2.1.0
  header[8]=0x02; header[9]=0x10;
  // class: mntr
  [...'mntr'].forEach((c,i)=>header[12+i]=c.charCodeAt(0));
  // color space: RGB
  [...'RGB '].forEach((c,i)=>header[16+i]=c.charCodeAt(0));
  // PCS: XYZ
  [...'XYZ '].forEach((c,i)=>header[20+i]=c.charCodeAt(0));
  // platform: APPL
  [...'APPL'].forEach((c,i)=>header[40+i]=c.charCodeAt(0));
  // primary platform: acsp
  [...'acsp'].forEach((c,i)=>header[36+i]=c.charCodeAt(0));
  // D65 illuminant
  setU32(header,68,0x0000F6D6); setU32(header,72,0x00010000); setU32(header,76,0x0000D32D);

  // Assemble
  const out = [...header, ...u32(numTags)];
  tags.forEach((t,i) => {
    out.push(...[...t.sig].map(c=>c.charCodeAt(0)));
    out.push(...u32(offsets[i]));
    out.push(...u32(t.data.length));
  });
  tags.forEach(t => out.push(...t.data));

  // Fix size
  out[0]=(totalSize>>>24)&0xFF; out[1]=(totalSize>>>16)&0xFF;
  out[2]=(totalSize>>>8)&0xFF; out[3]=totalSize&0xFF;

  return new Uint8Array(out);
}

const ADOBE_RGB_ICC = buildAdobeRGBICC();

// ─── Embed ICC profile into JPEG ─────────────────────────────────────────────
async function embedICC(jpegBlob, iccProfile) {
  const buf = await jpegBlob.arrayBuffer();
  const jpeg = new Uint8Array(buf);
  const marker = new TextEncoder().encode('ICC_PROFILE\0');
  const maxChunk = 65519 - marker.length - 2;
  const chunks = [];
  const total = Math.ceil(iccProfile.length / maxChunk);
  for (let i = 0; i < total; i++) {
    const slice = iccProfile.slice(i * maxChunk, (i+1) * maxChunk);
    const segLen = 2 + marker.length + 2 + slice.length;
    const seg = new Uint8Array(2 + segLen);
    seg[0]=0xFF; seg[1]=0xE2;
    seg[2]=(segLen>>8)&0xFF; seg[3]=segLen&0xFF;
    seg.set(marker, 4);
    seg[4+marker.length] = i+1;
    seg[5+marker.length] = total;
    seg.set(slice, 6+marker.length);
    chunks.push(seg);
  }
  const parts = [jpeg.slice(0,2), ...chunks, jpeg.slice(2)];
  return new Blob(parts, { type: 'image/jpeg' });
}

// ─── Color space transforms ───────────────────────────────────────────────────
function sRGBToLinear(c) { return c <= 0.04045 ? c/12.92 : Math.pow((c+0.055)/1.055, 2.4); }
function adobeGamma(c) { return Math.pow(Math.max(0,c), 1/2.19921875); }
const M_SRGB_XYZ = [[0.4124564,0.3575761,0.1804375],[0.2126729,0.7151522,0.072175],[0.0193339,0.119192,0.9503041]];
const M_XYZ_ADOBE = [[2.041369,-0.5649464,-0.3446944],[-0.969266,1.8760108,0.041556],[0.0134474,-0.1183897,1.0154096]];
const M_XYZ_PP = [[1.3459433,-0.2556075,-0.0511118],[-0.5445989,1.5081673,0.0205351],[0,0,1.2118128]];

function mul3(m,[r,g,b]) { return [m[0][0]*r+m[0][1]*g+m[0][2]*b, m[1][0]*r+m[1][1]*g+m[1][2]*b, m[2][0]*r+m[2][1]*g+m[2][2]*b]; }

function applyColorSpace(canvas, ctx, cs) {
  if (cs === 'srgb') return;
  const {width:w, height:h} = canvas;
  const id = ctx.getImageData(0,0,w,h);
  const d = id.data;
  for (let i = 0; i < d.length; i += 4) {
    const rl = sRGBToLinear(d[i]/255), gl = sRGBToLinear(d[i+1]/255), bl = sRGBToLinear(d[i+2]/255);
    const xyz = mul3(M_SRGB_XYZ, [rl,gl,bl]);
    let out;
    if (cs === 'adobe-rgb') {
      out = mul3(M_XYZ_ADOBE, xyz).map(v => Math.round(adobeGamma(v)*255));
    } else {
      out = mul3(M_XYZ_PP, xyz).map(v => Math.round(Math.pow(Math.max(0,v),1/1.8)*255));
    }
    d[i]   = Math.min(255,Math.max(0,out[0]));
    d[i+1] = Math.min(255,Math.max(0,out[1]));
    d[i+2] = Math.min(255,Math.max(0,out[2]));
  }
  ctx.putImageData(id,0,0);
}

// ─── Processing ───────────────────────────────────────────────────────────────
const canvas = document.getElementById('workCanvas');
const ctx = canvas.getContext('2d');

async function processFile(id) {
  const entry = files.get(id);
  if (!entry) return;
  setItemState(id, 'processing', 'Reading…');
  setProgress(id, 10);

  const quality = parseInt(document.getElementById('quality').value) / 100;
  const targetCS = document.getElementById('colorSpace').value;
  const sizeTarget = document.getElementById('outputSize').value;

  try {
    const buf = await entry.file.arrayBuffer();
    setProgress(id, 30);
    setItemState(id, 'processing', 'Extracting preview…');

    const thumbBlob = await extractJpegFromRAW(buf);
    if (!thumbBlob) throw new Error('No JPEG preview found in this RAW file. File may be corrupted or an unsupported variant.');

    setProgress(id, 55);
    setItemState(id, 'processing', 'Processing…');

    // Validate blob is decodable before committing
    let bmp;
    try {
      bmp = await createImageBitmap(thumbBlob);
    } catch(e) {
      throw new Error(`Extracted data isn\'t a valid JPEG (${Math.round(thumbBlob.size/1024)}KB found). The RAW preview may be HEIF or CR3 CRAW-encoded — needs native tools.`);
    }

    setProgress(id, 68);
    let w = bmp.width, h = bmp.height;
    if (sizeTarget !== 'original') {
      const max = parseInt(sizeTarget);
      const long = Math.max(w, h);
      if (long > max) { const s = max/long; w=Math.round(w*s); h=Math.round(h*s); }
    }
    canvas.width = w; canvas.height = h;
    ctx.drawImage(bmp, 0, 0, w, h);
    bmp.close();

    setProgress(id, 78);
    applyColorSpace(canvas, ctx, targetCS);
    setProgress(id, 88);

    const jpegBlob = await new Promise(res => canvas.toBlob(res, 'image/jpeg', quality));
    const finalBlob = targetCS === 'adobe-rgb' ? await embedICC(jpegBlob, ADOBE_RGB_ICC) : jpegBlob;
    setProgress(id, 100);

    const outName = entry.file.name.replace(/\.[^.]+$/, '') + '.jpg';
    const e = files.get(id);
    e.blob = finalBlob; e.outName = outName; e.state = 'done';

    setItemState(id, 'done', `Done · ${(finalBlob.size/1048576).toFixed(1)}MB`, finalBlob, outName);
  } catch(e) {
    setItemState(id, 'error', e.message);
  }
  updateActionBar();
}

// ─── Queue UI ─────────────────────────────────────────────────────────────────
function setProgress(id, pct) {
  const el = document.querySelector(`[data-id="${id}"] .progress-fill`);
  if (el) el.style.width = pct + '%';
}

function setItemState(id, state, label, blob, outName) {
  const item = document.querySelector(`[data-id="${id}"]`);
  if (!item) return;
  item.className = 'queue-item ' + state;
  const statusSpan = item.querySelector('.status-label');
  if (statusSpan) {
    if (state === 'error') {
      statusSpan.innerHTML = `<span style="color:var(--danger);font-size:10px;max-width:140px;word-break:break-word;white-space:normal;text-align:right;">${label}</span>`;
    } else {
      statusSpan.textContent = label;
    }
  }
  const slot = item.querySelector('.action-slot');
  if (state === 'done' && blob && slot) {
    const url = URL.createObjectURL(blob);
    slot.innerHTML = `<a class="download-btn" href="${url}" download="${outName}">
      <svg width="11" height="11" viewBox="0 0 14 14" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M7 2v7M4 6l3 3 3-3M2 11h10"/></svg>Save
    </a>`;
  }
  if (state !== 'processing') {
    const bar = item.querySelector('.progress-bar');
    if (bar) bar.style.opacity = state === 'done' ? '0' : '0.4';
  }
  if (files.get(id)) files.get(id).state = state;
}

function addToQueue(file) {
  const id = ++counter;
  files.set(id, { file, state: 'pending', blob: null });
  const ext = file.name.split('.').pop().toUpperCase();
  const sizeMB = (file.size/1048576).toFixed(1);
  const item = document.createElement('div');
  item.className = 'queue-item';
  item.dataset.id = id;
  item.innerHTML = `
    <div class="file-icon" id="thumb-${id}">${ext}</div>
    <div class="file-info">
      <div class="file-name">${file.name}</div>
      <div class="file-meta"><span>${sizeMB} MB</span><span>${ext}</span></div>
      <div class="progress-bar"><div class="progress-fill"></div></div>
    </div>
    <div class="file-status">
      <span class="status-dot"></span>
      <span class="status-label">Queued</span>
    </div>
    <div class="action-slot"></div>
    <button class="remove-btn" onclick="removeItem(${id})" title="Remove">×</button>`;
  document.getElementById('queue').appendChild(item);
  document.getElementById('emptyState').style.display = 'none';
  generateThumb(file, id);
  updateActionBar();
}

async function generateThumb(file, id) {
  try {
    const buf = await file.arrayBuffer();
    const blob = await extractJpegFromRAW(buf);
    if (!blob) return;
    const url = URL.createObjectURL(blob);
    const el = document.getElementById('thumb-' + id);
    if (el) el.innerHTML = `<img src="${url}" alt="">`;
  } catch(e) {}
}

function removeItem(id) {
  files.delete(id);
  const el = document.querySelector(`[data-id="${id}"]`);
  if (el) el.remove();
  if (files.size === 0) document.getElementById('emptyState').style.display = 'flex';
  updateActionBar();
}

function updateActionBar() {
  const hasPending = [...files.values()].some(f => f.state === 'pending');
  const hasDone = [...files.values()].some(f => f.state === 'done' && f.blob);
  document.getElementById('convertBtn').disabled = !hasPending;
  document.getElementById('downloadAllBtn').disabled = !hasDone;
}

async function convertAll() {
  const pending = [...files.entries()].filter(([,v]) => v.state === 'pending');
  document.getElementById('convertBtn').disabled = true;
  for (const [id] of pending) await processFile(id);
  updateActionBar();
}

async function downloadAll() {
  const done = [...files.values()].filter(f => f.state === 'done' && f.blob);
  for (const f of done) {
    const a = document.createElement('a');
    a.href = URL.createObjectURL(f.blob); a.download = f.outName; a.click();
    await new Promise(r => setTimeout(r, 200));
  }
}

// ─── Drop zone ────────────────────────────────────────────────────────────────
const dropZone = document.getElementById('dropZone');
const fileInput = document.getElementById('fileInput');
dropZone.addEventListener('click', e => { if (!e.target.closest('.browse-btn')) fileInput.click(); });
dropZone.addEventListener('dragover', e => { e.preventDefault(); dropZone.classList.add('drag-over'); });
dropZone.addEventListener('dragleave', () => dropZone.classList.remove('drag-over'));
dropZone.addEventListener('drop', e => { e.preventDefault(); dropZone.classList.remove('drag-over'); handleFiles(e.dataTransfer.files); });
fileInput.addEventListener('change', e => handleFiles(e.target.files));

function handleFiles(list) {
  Array.from(list).forEach(f => {
    if (/\.(cr2|cr3|crw|raf|nef|arw|rw2|dng)$/i.test(f.name) || f.type.startsWith('image/')) addToQueue(f);
  });
}

document.getElementById('quality').addEventListener('input', e => {
  document.getElementById('qualityVal').textContent = e.target.value + '%';
});
</script>
</body>
</html>
