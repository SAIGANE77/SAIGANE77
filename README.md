import { ExperienceTimeline } from "@/components/ExperienceTimeline";
import { GlitchName } from "@/components/GlitchName";
import { ParticleCanvas } from "@/components/ParticleCanvas";
import { ProjectCarousel } from "@/components/ProjectCarousel";
import { SkillRadar } from "@/components/SkillRadar";
import { Button } from "@/components/ui/button";
import { useAppState } from "@/context/AppContext";
import { generateMarkdown } from "@/lib/readmeContent";
import { THEMES } from "@/lib/themes";
import { Check, Code2, Copy, Eye } from "lucide-react";
import { motion } from "motion/react";
import { useMemo, useRef, useState } from "react";

const GITHUB = "https://github.com/kannurimeraganeshkumar";
const LINKEDIN = "https://www.linkedin.com/in/kannuri-mera-ganesh-kumar";
const LEETCODE = "https://leetcode.com/kannurimeraganeshkumar";
const MAILTO = "mailto:kannurimeraganesh@gmail.com";

function SectionDivider({ color }: { color: string }) {
  return (
    <div
      className="my-6 h-px"
      style={{
        background: `linear-gradient(90deg, transparent, ${color}44, transparent)`,
      }}
    />
  );
}

function SectionTitle({
  children,
  color,
}: { children: React.ReactNode; color: string }) {
  return (
    <h2
      className="text-xl font-bold font-display mb-4 pb-2 flex items-center gap-2"
      style={{
        color: "#c9d1d9",
        borderBottom: `1px solid ${color}33`,
      }}
    >
      {children}
    </h2>
  );
}

function BadgeLink({
  src,
  alt,
  href,
}: { src: string; alt: string; href?: string }) {
  const img = (
    <img
      src={src}
      alt={alt}
      loading="lazy"
      className="inline-block h-7 align-middle transition-transform duration-200 hover:scale-105"
    />
  );
  if (href) {
    return (
      <a
        href={href}
        target="_blank"
        rel="noopener noreferrer"
        className="inline-block mr-1 mb-1"
        data-ocid={`social.${alt.toLowerCase().replace(/\s+/g, "_")}_link`}
      >
        {img}
      </a>
    );
  }
  return <span className="inline-block mr-1 mb-1">{img}</span>;
}

function fadeUp(delay: number) {
  return {
    initial: { opacity: 0, y: 24 },
    whileInView: { opacity: 1, y: 0 },
    viewport: { once: true, margin: "-40px" },
    transition: { duration: 0.55, delay, ease: "easeOut" as const },
  };
}

const CERTS = [
  { icon: "🏆", name: "Google AI Course", issuer: "Google" },
  { icon: "📊", name: "Deloitte Data Analytics", issuer: "Deloitte" },
  { icon: "💻", name: "GFG DSA 160 Days Challenge", issuer: "GeeksforGeeks" },
];

export function ReadmePreview() {
  const { state } = useAppState();
  const containerRef = useRef<HTMLDivElement>(null);
  const [statsHovered, setStatsHovered] = useState(false);
  const [confettiBurst, setConfettiBurst] = useState(false);
  const [activeTab, setActiveTab] = useState<"preview" | "raw">("preview");
  const [copied, setCopied] = useState(false);
  const t = THEMES[state.theme];
  const previewBg = state.previewMode === "dark" ? "#0d1117" : "#ffffff";
  const previewCardBg = state.previewMode === "dark" ? "#161b22" : "#f6f8fa";
  const previewBorderCol = state.previewMode === "dark" ? "#30363d" : "#d0d7de";
  const textPrimary = state.previewMode === "dark" ? "#c9d1d9" : "#24292f";
  const textMuted = state.previewMode === "dark" ? "#8b949e" : "#57606a";
  const textBlue = state.previewMode === "dark" ? "#58a6ff" : "#0969da";

  const rawMarkdownText = useMemo(
    () =>
      generateMarkdown({
        theme: state.theme,
        name: state.name,
        bio: state.bio,
        roles: state.roles,
        sections: state.sections,
      }),
    [state.theme, state.name, state.bio, state.roles, state.sections],
  );

  const handleCopy = async () => {
    await navigator.clipboard.writeText(rawMarkdownText);
    setCopied(true);
    setTimeout(() => setCopied(false), 2200);
  };

  const handleNameClick = () => {
    setConfettiBurst(true);
    launchCSSConfetti(t.primary, t.secondary);
    setTimeout(() => setConfettiBurst(false), 2200);
  };

  return (
    <div
      ref={containerRef}
      className="relative w-full max-w-3xl mx-auto px-3 py-6"
      data-ocid="readme.preview"
    >
      {/* Particle trail */}
      <ParticleCanvas theme={state.theme} containerRef={containerRef} />

      {/* Confetti container */}
      {confettiBurst && (
        <ConfettiOverlay primary={t.primary} secondary={t.secondary} />
      )}

      <div
        className="relative rounded-xl overflow-hidden shadow-2xl z-20"
        style={{
          border: `1px solid ${previewBorderCol}`,
          background: previewBg,
        }}
        data-ocid="readme.card"
      >
        {/* macOS-style header bar */}
        <div
          className="flex items-center gap-2 px-4 py-2.5 border-b"
          style={{ background: previewCardBg, borderColor: previewBorderCol }}
        >
          <div className="flex gap-1.5">
            <span className="w-3 h-3 rounded-full bg-[#f85149] block" />
            <span className="w-3 h-3 rounded-full bg-[#d29922] block" />
            <span className="w-3 h-3 rounded-full bg-[#3fb950] block" />
          </div>
          <span className="font-mono text-xs ml-2" style={{ color: textMuted }}>
            README.md
          </span>
          <div className="ml-auto flex items-center gap-2">
            <span
              className="text-xs font-mono px-2 py-0.5 rounded-full"
              style={{
                background: `${t.primary}22`,
                color: t.primary,
                border: `1px solid ${t.primary}44`,
              }}
            >
              {state.theme}
            </span>
          </div>
        </div>

        {/* Tab bar */}
        <div
          className="flex items-center border-b px-4"
          style={{ background: previewCardBg, borderColor: previewBorderCol }}
        >
          <button
            type="button"
            onClick={() => setActiveTab("preview")}
            className="flex items-center gap-1.5 text-xs font-medium px-3 py-2.5 transition-colors duration-150"
            style={{
              color: activeTab === "preview" ? t.primary : textMuted,
              borderBottom:
                activeTab === "preview"
                  ? `2px solid ${t.primary}`
                  : "2px solid transparent",
            }}
            data-ocid="readme.preview_tab"
          >
            <Eye size={13} />
            Preview
          </button>
          <button
            type="button"
            onClick={() => setActiveTab("raw")}
            className="flex items-center gap-1.5 text-xs font-medium px-3 py-2.5 transition-colors duration-150"
            style={{
              color: activeTab === "raw" ? t.primary : textMuted,
              borderBottom:
                activeTab === "raw"
                  ? `2px solid ${t.primary}`
                  : "2px solid transparent",
            }}
            data-ocid="readme.raw_tab"
          >
            <Code2 size={13} />
            Raw Markdown
          </button>
          <div className="ml-auto">
            <Button
              type="button"
              size="sm"
              variant="ghost"
              className="h-7 text-xs gap-1.5 font-mono"
              style={{
                color: copied ? "#3fb950" : t.primary,
              }}
              onClick={handleCopy}
              data-ocid="readme.copy_button"
            >
              {copied ? <Check size={12} /> : <Copy size={12} />}
              {copied ? "Copied!" : "Copy README"}
            </Button>
          </div>
        </div>

        {activeTab === "raw" ? (
          /* ── Raw Markdown Tab ───────────────────────────────────── */
          <div className="relative" data-ocid="readme.raw_section">
            <div className="absolute top-3 right-3 z-10">
              <Button
                type="button"
                size="sm"
                onClick={handleCopy}
                className="text-xs gap-1.5 h-7"
                style={{
                  background: copied ? "#238636" : t.primary,
                  color: "#ffffff",
                  border: "none",
                }}
                data-ocid="readme.raw_copy_button"
              >
                {copied ? <Check size={12} /> : <Copy size={12} />}
                {copied ? "Copied!" : "Copy all"}
              </Button>
            </div>
            <pre
              className="overflow-auto text-xs font-mono leading-relaxed p-5 pt-12"
              style={{
                background: "#0d1117",
                color: "#c9d1d9",
                maxHeight: "70vh",
                whiteSpace: "pre-wrap",
                wordBreak: "break-word",
              }}
              data-ocid="readme.raw_markdown"
            >
              {rawMarkdownText}
            </pre>
            <div
              className="px-5 py-3 flex items-center justify-between text-xs border-t"
              style={{
                background: "#161b22",
                borderColor: "#30363d",
                color: "#8b949e",
              }}
            >
              <span>
                📋 {rawMarkdownText.split("\n").length} lines ·{" "}
                {rawMarkdownText.length} chars
              </span>
              <span style={{ color: "#3fb950" }}>
                ✓ GitHub-compatible markdown
              </span>
            </div>
          </div>
        ) : (
          /* ── Visual Preview Tab ─────────────────────────────────── */
          <div
            className="px-6 py-6 space-y-0"
            style={{ color: textPrimary, fontSize: 14 }}
          >
            {/* Hero: Capsule header */}
            <motion.div {...fadeUp(0)} className="text-center mb-2">
              <a href={GITHUB} target="_blank" rel="noopener noreferrer">
                <img
                  src={`https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=${t.capsuleColorList}&height=180&section=header&text=${encodeURIComponent(state.name)}&fontSize=36&fontColor=fff&animation=twinkling&fontAlignY=38&desc=${encodeURIComponent(state.bio)}&descAlignY=58&descSize=16`}
                  alt="Header"
                  loading="lazy"
                  className="w-full rounded-t-xl"
                />
              </a>
            </motion.div>

            {/* Glitch name + typing SVG */}
            <motion.div
              {...fadeUp(0.08)}
              className="text-center py-3"
              data-ocid="readme.hero_section"
            >
              <GlitchName
                name={state.name}
                theme={state.theme}
                onConfetti={handleNameClick}
              />
              <div className="mt-3">
                <a href={GITHUB} target="_blank" rel="noopener noreferrer">
                  <img
                    src={`https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=22&duration=3000&pause=1000&color=${t.primary.replace("#", "")}&center=true&vCenter=true&multiline=false&width=600&lines=${state.roles.map((r) => encodeURIComponent(r)).join(";")}`}
                    alt="Typing SVG"
                    loading="lazy"
                    className="mx-auto"
                  />
                </a>
              </div>
              <div className="mt-2">
                <img
                  src="https://komarev.com/ghpvc/?username=kannurimeraganeshkumar&label=Profile%20Views&color=0e75b6&style=for-the-badge"
                  alt="Profile Views"
                  loading="lazy"
                  className="inline-block"
                />
              </div>
            </motion.div>

            <SectionDivider color={t.primary} />

            {/* About Me */}
            <motion.section {...fadeUp(0.12)} data-ocid="readme.about_section">
              <SectionTitle color={t.primary}>👨‍💻 About Me</SectionTitle>
              <blockquote
                className="pl-4 my-3 italic text-sm"
                style={{
                  borderLeft: `3px solid ${t.primary}`,
                  color: textMuted,
                }}
              >
                B.Tech CSE (AI) | CGPA 8.28/10 | Kalasalingam Academy of
                Research and Education (2020–2026)
              </blockquote>
              <ul className="space-y-1.5 text-sm list-none">
                {[
                  {
                    emoji: "🌍",
                    text: "Based in",
                    bold: "Andhra Pradesh, India",
                  },
                  {
                    emoji: "🤖",
                    text: "Passionate about",
                    bold: "AI/ML, Backend Development",
                  },
                  {
                    emoji: "🎯",
                    text: "Solved",
                    bold: "400+ DSA problems",
                    suffix: "across LeetCode, GFG, and HackerRank",
                  },
                  {
                    emoji: "💡",
                    text: "Built projects across",
                    bold: "intelligent systems, recommendation engines",
                  },
                ].map((item) => (
                  <li key={item.emoji} className="flex gap-2">
                    {item.emoji} {item.text}{" "}
                    <strong style={{ color: textPrimary }}>{item.bold}</strong>
                    {item.suffix ? ` ${item.suffix}` : ""}
                  </li>
                ))}
                <li className="flex gap-2">
                  💫{" "}
                  <span style={{ color: textMuted }}>
                    kannurimeraganesh@gmail.com | 8341478388
                  </span>
                </li>
              </ul>
              <div className="mt-4 flex flex-wrap gap-1">
                <BadgeLink
                  src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"
                  alt="LinkedIn"
                  href={LINKEDIN}
                />
                <BadgeLink
                  src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white"
                  alt="GitHub"
                  href={GITHUB}
                />
                <BadgeLink
                  src="https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=LeetCode&logoColor=black"
                  alt="LeetCode"
                  href={LEETCODE}
                />
                <BadgeLink
                  src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"
                  alt="Gmail"
                  href={MAILTO}
                />
              </div>
            </motion.section>

            {state.sections.skills && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.14)}
                  data-ocid="readme.skills_section"
                >
                  <SectionTitle color={t.primary}>🚀 Tech Arsenal</SectionTitle>
                  <div
                    className="relative mt-2 rounded-2xl px-4 py-5"
                    style={{
                      background: "rgba(255,255,255,0.025)",
                      border: `1px solid ${t.primary}22`,
                      backdropFilter: "blur(8px)",
                    }}
                  >
                    <SkillRadar theme={state.theme} />
                  </div>
                </motion.section>
              </>
            )}

            {state.sections.stats && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.18)}
                  data-ocid="readme.stats_section"
                  onMouseEnter={() => setStatsHovered(true)}
                  onMouseLeave={() => setStatsHovered(false)}
                >
                  <SectionTitle color={t.primary}>📊 GitHub Stats</SectionTitle>
                  {statsHovered && (
                    <motion.p
                      initial={{ opacity: 0, y: -8 }}
                      animate={{ opacity: 1, y: 0 }}
                      className="text-xs mb-3 italic"
                      style={{ color: t.primary }}
                    >
                      Fun fact: powered by 9,999+ keystrokes ☕
                    </motion.p>
                  )}
                  <div className="flex flex-col items-center gap-3">
                    {[
                      {
                        src: "https://github-readme-stats.vercel.app/api?username=kannurimeraganeshkumar&show_icons=true&theme=github_dark&hide_border=true&count_private=true",
                        alt: "GitHub Stats",
                      },
                      {
                        src: "https://streak-stats.demolab.com?user=kannurimeraganeshkumar&theme=github-dark-blue&hide_border=true&date_format=M%20j%5B%2C%20Y%5D",
                        alt: "GitHub Streak",
                      },
                      {
                        src: "https://github-readme-stats.vercel.app/api/top-langs/?username=kannurimeraganeshkumar&layout=compact&theme=github_dark&hide_border=true&langs_count=8",
                        alt: "Top Languages",
                      },
                    ].map(({ src, alt }) => (
                      <img
                        key={alt}
                        src={src}
                        alt={alt}
                        loading="lazy"
                        className="max-w-full rounded-lg"
                      />
                    ))}
                  </div>
                </motion.section>
              </>
            )}

            {state.sections.snake && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.2)}
                  data-ocid="readme.snake_section"
                >
                  <SectionTitle color={t.primary}>
                    🐍 Contribution Graph
                  </SectionTitle>
                  <div className="flex justify-center py-2">
                    <img
                      src="https://raw.githubusercontent.com/platane/snk/output/github-contribution-grid-snake-dark.svg"
                      alt="Snake animation"
                      loading="lazy"
                      className="max-w-full rounded-lg"
                    />
                  </div>
                </motion.section>
              </>
            )}

            {state.sections.projects && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.22)}
                  data-ocid="readme.projects_section"
                >
                  <SectionTitle color={t.primary}>🚀 Projects</SectionTitle>
                  <ProjectCarousel theme={state.theme} previewBg={previewBg} />
                </motion.section>
              </>
            )}

            {state.sections.experience && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.24)}
                  data-ocid="readme.experience_section"
                >
                  <SectionTitle color={t.primary}>💼 Experience</SectionTitle>
                  <ExperienceTimeline
                    theme={state.theme}
                    previewBg={previewBg}
                  />
                </motion.section>
              </>
            )}

            {state.sections.education && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.26)}
                  data-ocid="readme.education_section"
                >
                  <SectionTitle color={t.primary}>🎓 Education</SectionTitle>
                  <div
                    className="rounded-xl p-4"
                    style={{
                      background: previewCardBg,
                      border: `1px solid ${previewBorderCol}`,
                    }}
                  >
                    <div className="flex items-start gap-3">
                      <span className="text-2xl">🎓</span>
                      <div>
                        <p
                          className="font-bold font-display"
                          style={{ color: textPrimary }}
                        >
                          B.Tech Computer Science Engineering (AI)
                        </p>
                        <p className="text-sm" style={{ color: textBlue }}>
                          Kalasalingam Academy of Research and Education
                        </p>
                        <div className="flex gap-4 mt-1">
                          <span
                            className="text-xs font-mono"
                            style={{ color: t.primary }}
                          >
                            CGPA: 8.28/10
                          </span>
                          <span
                            className="text-xs"
                            style={{ color: textMuted }}
                          >
                            2020 – 2026
                          </span>
                        </div>
                      </div>
                    </div>
                  </div>
                </motion.section>
              </>
            )}

            {state.sections.certs && (
              <>
                <SectionDivider color={t.primary} />
                <motion.section
                  {...fadeUp(0.28)}
                  data-ocid="readme.certs_section"
                >
                  <SectionTitle color={t.primary}>
                    📜 Certifications
                  </SectionTitle>
                  <div className="space-y-2">
                    {CERTS.map((cert, i) => (
                      <motion.div
                        key={cert.name}
                        initial={{ opacity: 0, x: -16 }}
                        whileInView={{ opacity: 1, x: 0 }}
                        viewport={{ once: true }}
                        transition={{ delay: i * 0.08 }}
                        className="flex items-center gap-3 rounded-xl px-4 py-3 transition-all duration-200 hover:scale-[1.01]"
                        style={{
                          background: previewCardBg,
                          border: `1px solid ${previewBorderCol}`,
                        }}
                        onMouseEnter={(e) => {
                          (e.currentTarget as HTMLElement).style.borderColor =
                            `${t.primary}55`;
                        }}
                        onMouseLeave={(e) => {
                          (e.currentTarget as HTMLElement).style.borderColor =
                            previewBorderCol;
                        }}
                        data-ocid={`readme.cert.item.${i + 1}`}
                      >
                        <span className="text-xl">{cert.icon}</span>
                        <div>
                          <p
                            className="font-semibold text-sm"
                            style={{ color: textPrimary }}
                          >
                            {cert.name}
                          </p>
                          <p className="text-xs" style={{ color: textMuted }}>
                            {cert.issuer}
                          </p>
                        </div>
                      </motion.div>
                    ))}
                  </div>
                </motion.section>
              </>
            )}

            <SectionDivider color={t.primary} />

            {/* Footer wave */}
            <motion.div {...fadeUp(0.3)} className="text-center pt-2">
              <a href={GITHUB} target="_blank" rel="noopener noreferrer">
                <img
                  src={`https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=${t.capsuleColorList}&height=90&section=footer`}
                  alt="Footer"
                  loading="lazy"
                  className="w-full rounded-b-xl"
                />
              </a>
              <p
                className="text-xs mt-3 animate-pulse-soft"
                style={{ color: textMuted }}
              >
                ⭐ Star my repositories if you find them helpful!
              </p>
            </motion.div>
          </div>
        )}
      </div>
    </div>
  );
}

// CSS-only confetti overlay
function ConfettiOverlay({
  primary,
  secondary,
}: { primary: string; secondary: string }) {
  const pieces = Array.from({ length: 28 }, (_, i) => ({
    id: i,
    x: 10 + Math.random() * 80,
    delay: Math.random() * 0.5,
    color: i % 3 === 0 ? primary : i % 3 === 1 ? secondary : "#ffcc00",
    size: 4 + Math.random() * 6,
    dx: (Math.random() - 0.5) * 200,
    dy: -(100 + Math.random() * 200),
    rot: Math.random() * 720 - 360,
  }));
  return (
    <div className="absolute inset-0 pointer-events-none z-30 overflow-hidden">
      {pieces.map((p) => (
        <div
          key={p.id}
          className="absolute rounded-sm"
          style={
            {
              left: `${p.x}%`,
              top: "40%",
              width: p.size,
              height: p.size,
              background: p.color,
              animation: `confetti-burst 1.6s ${p.delay}s ease-out both`,
              "--dx": `${p.dx}px`,
              "--dy": `${p.dy}px`,
            } as React.CSSProperties
          }
        />
      ))}
    </div>
  );
}

export function launchCSSConfetti(_p: string, _s: string) {
  // handled by ConfettiOverlay component
}
