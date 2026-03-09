import { useEffect, useRef, useState } from "react";

export default function GitHubProfile() {
  const canvasRef = useRef(null);
  const [typed, setTyped] = useState("");
  const fullText = "Cryptology & Cybersecurity Student";

  useEffect(() => {
    let i = 0;
    const interval = setInterval(() => {
      if (i <= fullText.length) {
        setTyped(fullText.slice(0, i));
        i++;
      } else {
        clearInterval(interval);
      }
    }, 60);
    return () => clearInterval(interval);
  }, []);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;

    const chars = "01ABCDEF↯⊕⊗∇∆⟨⟩";
    const fontSize = 13;
    const cols = Math.floor(canvas.width / fontSize);
    const drops = Array(cols).fill(0).map(() => Math.random() * -50);

    const draw = () => {
      ctx.fillStyle = "rgba(0,0,0,0.05)";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      for (let i = 0; i < cols; i++) {
        const char = chars[Math.floor(Math.random() * chars.length)];
        const alpha = Math.random() * 0.5 + 0.1;
        ctx.fillStyle = `rgba(100,220,180,${alpha})`;
        ctx.font = `${fontSize}px monospace`;
        ctx.fillText(char, i * fontSize, drops[i] * fontSize);
        if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
        drops[i] += 0.4;
      }
    };

    const id = setInterval(draw, 40);
    return () => clearInterval(id);
  }, []);

  const skills = [
    { label: "FPGA Design", icon: "⬡", sub: "Cryptographic Circuits" },
    { label: "Post-Quantum", icon: "⟨ψ⟩", sub: "Circuit ABE" },
    { label: "Cryptography", icon: "🔐", sub: "Algorithms & Proofs" },
    { label: "Cybersecurity", icon: "⚔", sub: "Defense & Analysis" },
  ];

  return (
    <div style={{
      fontFamily: "'Courier New', monospace",
      background: "#090d0f",
      minHeight: "100vh",
      color: "#c8ffe8",
      display: "flex",
      flexDirection: "column",
      alignItems: "center",
      justifyContent: "center",
      padding: "2rem",
      position: "relative",
      overflow: "hidden",
    }}>
      {/* Matrix rain background */}
      <canvas ref={canvasRef} style={{
        position: "fixed", top: 0, left: 0,
        width: "100%", height: "100%",
        opacity: 0.35, zIndex: 0, pointerEvents: "none",
      }} />

      {/* Glow blobs */}
      {[
        { top: "10%", left: "5%", color: "rgba(0,200,130,0.08)" },
        { top: "60%", right: "5%", color: "rgba(80,120,255,0.07)" },
      ].map((b, i) => (
        <div key={i} style={{
          position: "fixed", width: 400, height: 400,
          borderRadius: "50%", filter: "blur(100px)",
          background: b.color, zIndex: 0,
          top: b.top, left: b.left, right: b.right,
          animation: `pulse ${3 + i}s ease-in-out infinite alternate`,
        }} />
      ))}

      <style>{`
        @keyframes pulse { from { transform: scale(1); } to { transform: scale(1.3); } }
        @keyframes fadeUp { from { opacity:0; transform:translateY(20px); } to { opacity:1; transform:translateY(0); } }
        @keyframes blink { 0%,100% { opacity:1; } 50% { opacity:0; } }
        @keyframes scanline { 0% { top: -2px; } 100% { top: 100%; } }
        .skill-card:hover { background: rgba(0,200,130,0.1) !important; transform: translateY(-3px); border-color: rgba(0,200,130,0.5) !important; }
        .stat-box:hover { background: rgba(0,200,130,0.08) !important; }
      `}</style>

      {/* Main card */}
      <div style={{
        position: "relative", zIndex: 1,
        maxWidth: 680, width: "100%",
        border: "1px solid rgba(0,200,130,0.25)",
        borderRadius: 4,
        background: "rgba(9,13,15,0.85)",
        backdropFilter: "blur(12px)",
        overflow: "hidden",
        animation: "fadeUp 0.8s ease both",
        boxShadow: "0 0 60px rgba(0,200,130,0.06), inset 0 0 40px rgba(0,0,0,0.4)",
      }}>
        {/* Scanline effect */}
        <div style={{
          position: "absolute", left: 0, right: 0, height: 2,
          background: "linear-gradient(transparent, rgba(0,200,130,0.15), transparent)",
          animation: "scanline 4s linear infinite", zIndex: 10, pointerEvents: "none",
        }} />

        {/* Header bar */}
        <div style={{
          padding: "10px 20px",
          borderBottom: "1px solid rgba(0,200,130,0.15)",
          display: "flex", alignItems: "center", gap: 8,
          background: "rgba(0,200,130,0.03)",
        }}>
          {["#ff5f56","#ffbd2e","#27c93f"].map((c,i) => (
            <div key={i} style={{ width:10, height:10, borderRadius:"50%", background:c, opacity:0.7 }} />
          ))}
          <span style={{ marginLeft: 8, fontSize: 11, opacity: 0.4, letterSpacing: 2 }}>PROFILE.md — ENCRYPTED</span>
        </div>

        <div style={{ padding: "2.2rem 2.5rem" }}>
          {/* Name block */}
          <div style={{ marginBottom: "1.8rem" }}>
            <div style={{ fontSize: 11, letterSpacing: 4, color: "#3dffc0", opacity: 0.6, marginBottom: 6 }}>
              &gt; IDENTIFY
            </div>
            <h1 style={{
              fontSize: "clamp(1.6rem,4vw,2.4rem)",
              margin: 0, letterSpacing: 2,
              background: "linear-gradient(135deg, #e8fff5 30%, #3dffc0)",
              WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent",
              fontWeight: 700,
            }}>
              [Your Name]
            </h1>
            <div style={{ fontSize: 14, marginTop: 6, color: "#3dffc0", letterSpacing: 1 }}>
              {typed}<span style={{ animation: "blink 1s step-end infinite", color:"#3dffc0" }}>_</span>
            </div>
          </div>

          {/* Bio */}
          <div style={{
            fontSize: 13.5, lineHeight: 1.85,
            color: "rgba(200,255,232,0.7)",
            borderLeft: "2px solid rgba(0,200,130,0.3)",
            paddingLeft: 16, marginBottom: "2rem",
          }}>
            Studying <span style={{ color:"#3dffc0" }}>Cryptology & Cybersecurity</span> with a focus on
            hardware-level cryptographic design. I build cryptographic circuits on
            <span style={{ color:"#3dffc0" }}> FPGAs</span> and research
            <span style={{ color:"#3dffc0" }}> post-quantum solutions</span> —
            particularly <span style={{ color:"#3dffc0" }}>Circuit Attribute-Based Encryption</span> for
            next-generation secure systems.
          </div>

          {/* Skills grid */}
          <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:10, marginBottom:"2rem" }}>
            {skills.map((s, i) => (
              <div key={i} className="skill-card" style={{
                padding: "14px 16px",
                border: "1px solid rgba(0,200,130,0.15)",
                borderRadius: 3,
                background: "rgba(0,200,130,0.03)",
                transition: "all 0.25s ease",
                cursor: "default",
                animation: `fadeUp 0.6s ease ${0.2 + i * 0.1}s both`,
              }}>
                <div style={{ fontSize: 20, marginBottom: 4 }}>{s.icon}</div>
                <div style={{ fontSize: 13, fontWeight: 700, letterSpacing: 1 }}>{s.label}</div>
                <div style={{ fontSize: 11, opacity: 0.45, marginTop: 2 }}>{s.sub}</div>
              </div>
            ))}
          </div>

          {/* Stats row */}
          <div style={{ display:"flex", gap:10, marginBottom:"1.8rem" }}>
            {[
              { val: "FPGA", desc: "Hardware Crypto" },
              { val: "PQC", desc: "Post-Quantum" },
              { val: "ABE", desc: "Access Control" },
            ].map((s,i) => (
              <div key={i} className="stat-box" style={{
                flex:1, textAlign:"center", padding:"12px 8px",
                border:"1px solid rgba(0,200,130,0.12)",
                borderRadius:3, background:"rgba(0,0,0,0.2)",
                transition:"background 0.2s",
                animation:`fadeUp 0.6s ease ${0.5+i*0.08}s both`,
              }}>
                <div style={{ fontSize:16, fontWeight:800, color:"#3dffc0", letterSpacing:2 }}>{s.val}</div>
                <div style={{ fontSize:10, opacity:0.4, marginTop:3, letterSpacing:1 }}>{s.desc}</div>
              </div>
            ))}
          </div>

          {/* Footer */}
          <div style={{
            fontSize:11, opacity:0.3, letterSpacing:2,
            borderTop:"1px solid rgba(0,200,130,0.1)", paddingTop:16,
            display:"flex", justifyContent:"space-between",
          }}>
            <span>SHA-256: 3a7f...c19e</span>
            <span>PGP VERIFIED ✓</span>
          </div>
        </div>
      </div>
    </div>
  );
}
