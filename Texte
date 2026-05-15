import { useState } from "react";

const TABS = [
  { id: "seo", label: "🔍 SEO", desc: "Titre & description optimisés" },
  { id: "social", label: "📲 Réseaux sociaux", desc: "Accroches par plateforme" },
  { id: "angles", label: "💡 Angles éditoriaux", desc: "Idées pour maximiser l'engagement" },
];

const PLATFORMS = ["YouTube", "Facebook", "X (Twitter)", "Instagram", "TikTok"];

async function callClaude(systemPrompt, userPrompt) {
  const response = await fetch("/.netlify/functions/generate", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ system: systemPrompt, prompt: userPrompt }),
  });
  const data = await response.json();
  return data.content?.[0]?.text || "";
}

function Spinner() {
  return (
    <div style={{ display: "flex", justifyContent: "center", padding: "2rem" }}>
      <div style={{
        width: 32, height: 32,
        border: "3px solid #f0e6d3",
        borderTop: "3px solid #c0392b",
        borderRadius: "50%",
        animation: "spin 0.8s linear infinite",
      }} />
      <style>{`@keyframes spin { to { transform: rotate(360deg); } }`}</style>
    </div>
  );
}

function ResultBlock({ text }) {
  const [copied, setCopied] = useState(false);
  const copy = () => {
    navigator.clipboard.writeText(text);
    setCopied(true);
    setTimeout(() => setCopied(false), 1500);
  };
  return (
    <div style={{
      background: "#1a0a00", border: "1px solid #3d1f00",
      borderRadius: 8, padding: "1.2rem 1.4rem",
      position: "relative", marginBottom: "1rem",
    }}>
      <pre style={{
        fontFamily: "'IBM Plex Mono', monospace", fontSize: "0.82rem",
        color: "#f0dfc0", whiteSpace: "pre-wrap", margin: 0, lineHeight: 1.7,
      }}>{text}</pre>
      <button onClick={copy} style={{
        position: "absolute", top: 10, right: 10,
        background: copied ? "#27ae60" : "#2c1500",
        border: "1px solid #5a3010", borderRadius: 4, color: "#f0dfc0",
        fontSize: "0.7rem", padding: "3px 10px",
        cursor: "pointer", transition: "background 0.3s",
      }}>
        {copied ? "✓ Copié" : "Copier"}
      </button>
    </div>
  );
}

export default function App() {
  const [activeTab, setActiveTab] = useState("seo");
  const [input, setInput] = useState("");
  const [selectedPlatforms, setSelectedPlatforms] = useState(["YouTube", "Facebook"]);
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const togglePlatform = (p) => {
    setSelectedPlatforms(prev =>
      prev.includes(p) ? prev.filter(x => x !== p) : [...prev, p]
    );
  };

  const handleGenerate = async () => {
    if (!input.trim()) return;
    setLoading(true); setResult(null); setError(null);
    const systemBase = `Tu es un expert en stratégie éditoriale et engagement pour les médias africains francophones, spécialisé dans les web TV. Tes réponses sont directes, pratiques, sans introductions inutiles. Tu écris en français.`;
    let system = systemBase, prompt = "";
    try {
      if (activeTab === "seo") {
        system += " Tu maîtrises le SEO YouTube et Google pour les médias vidéo.";
        prompt = `Sujet ou titre brut : "${input}"\n\nGénère :\n1. TITRE SEO (max 70 caractères, accrocheur, mot-clé fort en début)\n2. DESCRIPTION YOUTUBE (150-200 mots : accroche, résumé, mots-clés naturels, appel à l'action)\n3. TAGS RECOMMANDÉS (10-15 tags, séparés par des virgules)`;
      } else if (activeTab === "social") {
        prompt = `Sujet ou titre : "${input}"\nPlateformes cibles : ${selectedPlatforms.join(", ")}\n\nPour chaque plateforme, génère une accroche adaptée (ton, longueur, style propre à chaque réseau).\nFormat : [NOM PLATEFORME] suivi du texte.`;
      } else {
        prompt = `Sujet ou actualité : "${input}"\n\nPropose 4 angles éditoriaux originaux pour traiter ce sujet en web TV.\nPour chaque angle : un titre percutant + 2 lignes expliquant l'approche et pourquoi ça engage.\nNumérote-les clairement.`;
      }
      const text = await callClaude(system, prompt);
      setResult(text);
    } catch (e) {
      setError("Erreur lors de la génération. Veuillez réessayer.");
    } finally {
      setLoading(false);
    }
  };

  const placeholders = {
    seo: "Ex : Interview exclusive du maire de Dakar sur la hausse du coût de la vie",
    social: "Ex : Reportage sur les jeunes entrepreneurs de Saint-Louis",
    angles: "Ex : Élections locales prévues en décembre prochain",
  };

  return (
    <div style={{ minHeight: "100vh", background: "#0d0500", fontFamily: "'Playfair Display', Georgia, serif", color: "#f0dfc0", padding: "0 0 4rem" }}>
      <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=IBM+Plex+Mono:wght@400;500&family=Source+Sans+3:wght@400;600&display=swap" rel="stylesheet" />
      <div style={{ background: "linear-gradient(135deg, #1a0800 0%, #2c0d00 50%, #1a0800 100%)", borderBottom: "2px solid #8b3a00", padding: "2.5rem 2rem 2rem", textAlign: "center" }}>
        <div style={{ fontSize: "0.7rem", letterSpacing: "0.35em", color: "#c0392b", fontFamily: "'IBM Plex Mono', monospace", marginBottom: "0.5rem", textTransform: "uppercase" }}>OUTIL ÉDITORIAL</div>
        <h1 style={{ fontSize: "clamp(1.8rem, 4vw, 2.8rem)", fontWeight: 900, margin: "0 0 0.3rem", background: "linear-gradient(90deg, #f5d090, #e8a060, #f5d090)", WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent" }}>SIGNAL WEB TV</h1>
        <p style={{ fontSize: "0.85rem", color: "#9a7050", fontFamily: "'Source Sans 3', sans-serif", margin: 0 }}>Optimisez votre audience & votre engagement en quelques secondes</p>
      </div>
      <div style={{ maxWidth: 720, margin: "0 auto", padding: "2rem 1.5rem 0" }}>
        <div style={{ display: "flex", gap: "0.5rem", marginBottom: "1.8rem", flexWrap: "wrap" }}>
          {TABS.map(tab => (
            <button key={tab.id} onClick={() => { setActiveTab(tab.id); setResult(null); setError(null); }}
              style={{ flex: 1, minWidth: 140, padding: "0.8rem 1rem", background: activeTab === tab.id ? "#c0392b" : "#1a0800", border: activeTab === tab.id ? "1px solid #e74c3c" : "1px solid #3d1f00", borderRadius: 8, cursor: "pointer", color: activeTab === tab.id ? "#fff" : "#9a7050", fontFamily: "'Source Sans 3', sans-serif", fontWeight: 600, fontSize: "0.85rem", textAlign: "center" }}>
              <div>{tab.label}</div>
              <div style={{ fontSize: "0.7rem", fontWeight: 400, marginTop: 2, opacity: 0.8 }}>{tab.desc}</div>
            </button>
          ))}
        </div>
        {activeTab === "social" && (
          <div style={{ marginBottom: "1.2rem" }}>
            <div style={{ fontSize: "0.7rem", letterSpacing: "0.2em", color: "#7a5030", fontFamily: "'IBM Plex Mono', monospace", marginBottom: "0.6rem" }}>PLATEFORMES CIBLES</div>
            <div style={{ display: "flex", gap: "0.5rem", flexWrap: "wrap" }}>
              {PLATFORMS.map(p => (
                <button key={p} onClick={() => togglePlatform(p)}
                  style={{ padding: "0.4rem 1rem", background: selectedPlatforms.includes(p) ? "#3d1500" : "#120500", border: selectedPlatforms.includes(p) ? "1px solid #c0392b" : "1px solid #2c1000", borderRadius: 20, cursor: "pointer", color: selectedPlatforms.includes(p) ? "#f0a060" : "#5a3520", fontFamily: "'Source Sans 3', sans-serif", fontSize: "0.78rem", fontWeight: 600 }}>{p}</button>
              ))}
            </div>
          </div>
        )}
        <div style={{ marginBottom: "1.2rem" }}>
          <div style={{ fontSize: "0.7rem", letterSpacing: "0.2em", color: "#7a5030", fontFamily: "'IBM Plex Mono', monospace", marginBottom: "0.6rem" }}>VOTRE SUJET OU TITRE BRUT</div>
          <textarea value={input} onChange={e => setInput(e.target.value)} placeholder={placeholders[activeTab]} rows={3}
            style={{ width: "100%", boxSizing: "border-box", background: "#100500", border: "1px solid #3d1f00", borderRadius: 8, padding: "1rem 1.2rem", color: "#f0dfc0", fontSize: "0.92rem", fontFamily: "'Source Sans 3', sans-serif", resize: "vertical", lineHeight: 1.6, outline: "none" }} />
        </div>
        <button onClick={handleGenerate} disabled={loading || !input.trim()}
          style={{ width: "100%", padding: "1rem", background: loading || !input.trim() ? "#2c1000" : "linear-gradient(135deg, #c0392b, #922b21)", border: "none", borderRadius: 8, cursor: loading || !input.trim() ? "not-allowed" : "pointer", color: loading || !input.trim() ? "#5a3020" : "#fff", fontFamily: "'Source Sans 3', sans-serif", fontWeight: 700, fontSize: "1rem", marginBottom: "2rem" }}>
          {loading ? "Génération en cours…" : "✦ Générer"}
        </button>
        {loading && <Spinner />}
        {error && <div style={{ background: "#2c0000", border: "1px solid #7a0000", borderRadius: 8, padding: "1rem", color: "#ff6b6b", fontFamily: "'Source Sans 3', sans-serif", fontSize: "0.85rem" }}>{error}</div>}
        {result && !loading && (
          <div>
            <div style={{ fontSize: "0.7rem", letterSpacing: "0.2em", color: "#7a5030", fontFamily: "'IBM Plex Mono', monospace", marginBottom: "0.8rem" }}>RÉSULTAT GÉNÉRÉ</div>
            <ResultBlock text={result} />
          </div>
        )}
      </div>
    </div>
  );
}
