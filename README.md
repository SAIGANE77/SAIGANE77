import { THEMES } from "@/lib/themes";
import type { ThemePreset } from "@/types";

const GITHUB = "https://github.com/kannurimeraganeshkumar";
const LINKEDIN = "https://www.linkedin.com/in/kannuri-mera-ganesh-kumar";
const LEETCODE = "https://leetcode.com/kannurimeraganeshkumar";
const MAILTO = "mailto:kannurimeraganesh@gmail.com";

export function generateMarkdown(params: {
  theme: ThemePreset;
  name: string;
  bio: string;
  roles: string[];
  sections: Record<string, boolean>;
}): string {
  const { theme, name, bio, roles, sections } = params;
  const t = THEMES[theme];
  const headerColor = t.capsuleColorList;
  const lines: string[] = [];

  lines.push('<div align="center">');
  lines.push("");
  lines.push(
    `[![Header](https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=${headerColor}&height=200&section=header&text=${encodeURIComponent(name)}&fontSize=40&fontColor=fff&animation=twinkling&fontAlignY=35&desc=${encodeURIComponent(bio)}&descAlignY=55&descSize=18)](${GITHUB})`,
  );
  lines.push("");
  const typingLines = roles.map((r) => encodeURIComponent(r)).join(";");
  lines.push(
    `[![Typing SVG](https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=26&duration=3000&pause=1000&color=${t.primary.replace("#", "")}&center=true&vCenter=true&multiline=false&width=700&lines=${typingLines})](${GITHUB})`,
  );
  lines.push("");
  lines.push(
    "![Profile Views](https://komarev.com/ghpvc/?username=kannurimeraganeshkumar&label=Profile%20Views&color=0e75b6&style=for-the-badge)",
  );
  lines.push("");
  lines.push("</div>");
  lines.push("");
  lines.push("---");
  lines.push("");
  lines.push("## About Me");
  lines.push("");
  lines.push(
    "> **B.Tech CSE (AI) | CGPA 8.28/10 | Kalasalingam Academy of Research and Education (2020-2026)**",
  );
  lines.push("");
  lines.push("- Based in **Andhra Pradesh, India**");
  lines.push(
    "- Passionate about **AI/ML, Backend Development**, and solving real-world problems",
  );
  lines.push(
    "- Solved **400+ DSA problems** across LeetCode, GFG, and HackerRank",
  );
  lines.push(
    "- Built projects across **intelligent systems**, **recommendation engines**, and **full-stack apps**",
  );
  lines.push("- kannurimeraganesh@gmail.com | 8341478388");
  lines.push("");
  lines.push(
    `[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](${LINKEDIN})`,
  );
  lines.push(
    `[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](${GITHUB})`,
  );
  lines.push(
    `[![LeetCode](https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=LeetCode&logoColor=black)](${LEETCODE})`,
  );
  lines.push(
    `[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](${MAILTO})`,
  );
  lines.push("");
  lines.push("---");

  if (sections.skills !== false) {
    lines.push("");
    lines.push("## 🚀 Tech Arsenal");
    lines.push("");
    lines.push('<div align="center">');
    lines.push("");
    lines.push("**Languages**");
    lines.push("");
    lines.push(
      "[![Languages](https://skillicons.dev/icons?i=java,python,c,js,mysql&perline=6)](https://skillicons.dev)",
    );
    lines.push("");
    lines.push("**Frontend**");
    lines.push("");
    lines.push(
      "[![Frontend](https://skillicons.dev/icons?i=react,html,css&perline=6)](https://skillicons.dev)",
    );
    lines.push("");
    lines.push("**Backend & APIs**");
    lines.push("");
    lines.push(
      "[![Backend](https://skillicons.dev/icons?i=nodejs,express,postman&perline=6)](https://skillicons.dev)",
    );
    lines.push("");
    lines.push("**Databases**");
    lines.push("");
    lines.push(
      "[![Databases](https://skillicons.dev/icons?i=mongodb,mysql&perline=6)](https://skillicons.dev)",
    );
    lines.push("");
    lines.push("**AI / ML**");
    lines.push("");
    lines.push(
      "[![AI/ML](https://skillicons.dev/icons?i=tensorflow,sklearn,opencv&perline=6)](https://skillicons.dev)",
    );
    lines.push("");
    lines.push("**Tools & Platforms**");
    lines.push("");
    lines.push(
      "[![Tools](https://skillicons.dev/icons?i=git,aws,vscode,postman&perline=6)](https://skillicons.dev)",
    );
    lines.push("");
    lines.push("</div>");
    lines.push("");
    lines.push("---");
  }

  if (sections.stats !== false) {
    lines.push("");
    lines.push("## GitHub Stats");
    lines.push("");
    lines.push('<div align="center">');
    lines.push("");
    lines.push(
      "![GitHub Stats](https://github-readme-stats.vercel.app/api?username=kannurimeraganeshkumar&show_icons=true&theme=github_dark&hide_border=true&count_private=true)",
    );
    lines.push("");
    lines.push(
      "![GitHub Streak](https://streak-stats.demolab.com?user=kannurimeraganeshkumar&theme=github-dark-blue&hide_border=true&date_format=M%20j%5B%2C%20Y%5D)",
    );
    lines.push("");
    lines.push(
      "![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=kannurimeraganeshkumar&layout=compact&theme=github_dark&hide_border=true&langs_count=8)",
    );
    lines.push("");
    lines.push("</div>");
    lines.push("");
    lines.push("---");
  }

  if (sections.snake !== false) {
    lines.push("");
    lines.push("## Contribution Graph");
    lines.push("");
    lines.push('<div align="center">');
    lines.push("");
    lines.push(
      "![Snake animation](https://raw.githubusercontent.com/platane/snk/output/github-contribution-grid-snake-dark.svg)",
    );
    lines.push("");
    lines.push("</div>");
    lines.push("");
    lines.push("---");
  }

  if (sections.projects !== false) {
    lines.push("");
    lines.push("## Projects");
    lines.push("");
    lines.push("### Smart Inventory Management System");
    lines.push("> **Stack:** Node.js, Express.js, MongoDB, React.js");
    lines.push(
      "- Full-stack inventory system with CRUD operations, real-time stock tracking, and analytics dashboard",
    );
    lines.push(
      "- RESTful APIs with authentication, role-based access control, and automated reorder alerts",
    );
    lines.push(`- [View Project](${GITHUB})`);
    lines.push("");
    lines.push("### AI/ML-Based Skill Recommender");
    lines.push("> **Stack:** Python, Scikit-learn, Pandas, NLP");
    lines.push(
      "- Personalized skill recommendation engine using collaborative filtering and NLP techniques",
    );
    lines.push(
      "- Analyzes user profiles, job market trends, and skill gap analysis to recommend learning paths",
    );
    lines.push(`- [View Project](${GITHUB})`);
    lines.push("");
    lines.push("### Music Recommendation by Facial Expression");
    lines.push("> **Stack:** Python, TensorFlow, OpenCV, CNN");
    lines.push(
      "- Real-time emotion detection using CNN model with **92% accuracy**",
    );
    lines.push(
      "- Maps detected facial expressions to music moods and recommends tracks dynamically",
    );
    lines.push(`- [View Project](${GITHUB})`);
    lines.push("");
    lines.push("---");
  }

  if (sections.experience !== false) {
    lines.push("");
    lines.push("## Experience");
    lines.push("");
    lines.push("| Role | Company | Duration |");
    lines.push("|------|---------|----------|");
    lines.push(
      "| Software Development Engineer Intern | **Bluestock Fintech** | Oct 2025 - Nov 2025 |",
    );
    lines.push("| AI/ML Intern | **Rinex AI** | Sep 2024 - Oct 2024 |");
    lines.push(
      "| Web Development Intern | **Octanet** | Jun 2024 - Jul 2024 |",
    );
    lines.push("");
    lines.push("---");
  }

  if (sections.education !== false) {
    lines.push("");
    lines.push("## Education");
    lines.push("");
    lines.push("| Degree | Institution | CGPA | Year |");
    lines.push("|--------|------------|------|------|");
    lines.push(
      "| B.Tech CSE (AI) | Kalasalingam Academy of Research and Education | 8.28/10 | 2020-2026 |",
    );
    lines.push("");
    lines.push("---");
  }

  if (sections.certs !== false) {
    lines.push("");
    lines.push("## Certifications");
    lines.push("");
    lines.push("- **Google AI Course** - Google");
    lines.push("- **Deloitte Data Analytics** - Deloitte");
    lines.push("- **GFG DSA 160 Days Challenge** - GeeksforGeeks");
    lines.push("");
    lines.push("---");
  }

  lines.push("");
  lines.push('<div align="center">');
  lines.push("");
  lines.push(
    `[![Footer](https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=${headerColor}&height=100&section=footer)](${GITHUB})`,
  );
  lines.push("");
  lines.push("*Star my repositories if you find them helpful!*");
  lines.push("");
  lines.push("</div>");
  lines.push("");
  return lines.join("\n");
}

export const rawMarkdown = generateMarkdown({
  theme: "neon",
  name: "KANNURI MERA GANESH KUMAR",
  bio: "Software Engineer | AI/ML | Backend Dev",
  roles: [
    "Hi, I'm Ganesh Kumar",
    "AI/ML Engineer",
    "Backend Developer",
    "DSA Enthusiast (400+ Problems)",
    "Building Smart Solutions",
  ],
  sections: {
    skills: true,
    projects: true,
    experience: true,
    education: true,
    certs: true,
    stats: true,
    snake: true,
  },
});
