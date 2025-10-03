'use client';

import React, { useMemo, useState } from "react";
import { motion } from "framer-motion";
import { PieChart, Pie, Cell, ResponsiveContainer, Tooltip, Legend } from "recharts";
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";
import { Accordion, AccordionContent, AccordionItem, AccordionTrigger } from "@/components/ui/accordion";
import { Badge } from "@/components/ui/badge";
import {
  CheckCircle2, Download, Leaf, LineChart, ShieldCheck, Wallet,
  Globe2, Link as LinkIcon, ArrowRight, Sparkles, Building2, Users2
} from "lucide-react";

// -------------------------------------------------------------
// VERDAX (VDX) — Single-file, production-ready landing page
// Styling: TailwindCSS (assumed available)
// Components: shadcn/ui, lucide-react, framer-motion, recharts
// Notes:
// - Replace placeholder links (whitepaper URL, swap/staking widgets, socials)
// - Hook up newsletter form to your provider (e.g., Mailchimp, Brevo, ConvertKit)
// - Integrate real price feed via your widget/provider
// -------------------------------------------------------------

const tokenomics = [
  { name: "Public Sale", value: 40 },
  { name: "Strategic Partners", value: 25 },
  { name: "Community & Sustainability", value: 20 },
  { name: "Team (Vesting)", value: 10 },
  { name: "Liquidity Reserve", value: 5 },
];

const roadmap = [
  {
    phase: "Phase 1 — Foundation (2025)",
    items: [
      "Whitepaper, legal strategy, brand launch",
      "Private + public presale",
      "Website v1 + staking portal (MVP)",
    ],
  },
  {
    phase: "Phase 2 — Expansion (2026)",
    items: [
      "Tier-1/2 exchange listings",
      "Resource tokenization pilots (gold & cobalt)",
      "Gov & mining partnerships across Africa",
    ],
  },
  {
    phase: "Phase 3 — Impact (2027–2028)",
    items: [
      "Wallet & remittance app",
      "Community fund deployments (schools, renewables)",
      "Verdax Foundation & on-chain transparency hub",
    ],
  },
];

const benefits = [
  {
    title: "Resource-Backed Value",
    desc: "Designed to be backed by audited mineral reserves (gold, cobalt, copper, diamonds) for long-term stability.",
    icon: ShieldCheck,
  },
  {
    title: "Sustainability at Core",
    desc: "A portion of proceeds fuels environmental projects and community development across Africa.",
    icon: Leaf,
  },
  {
    title: "Global Access",
    desc: "Simple access for the African diaspora and global investors via on-ramps, DEX, and CEX listings.",
    icon: Globe2,
  },
  {
    title: "DeFi Utility",
    desc: "Staking, governance, and cross-border payments in a growing ecosystem of apps.",
    icon: Wallet,
  },
];

const faqs = [
  {
    q: "What makes Verdax different from other cryptocurrencies?",
    a: "Verdax is designed to be backed by audited mineral reserves and to reinvest in sustainability and community impact, blending real-world asset exposure with Web3 utility.",
  },
  {
    q: "When can I buy VDX?",
    a: "During the presale window, via the integrated swap widget, or on supported exchanges after listing. Timelines are announced on our official channels.",
  },
  {
    q: "Is Verdax audited?",
    a: "Smart-contract and reserve attestation audits are planned prior to mainnet launch and will be published on this site.",
  },
  {
    q: "Which chain does Verdax use?",
    a: "VDX can deploy on EVM-compatible networks. Final chain(s) and bridges will be announced with technical documentation.",
  },
];

// ----- Small UI helpers -----
const Stat = ({ label, value }: { label: string; value: string }) => (
  <div className="flex flex-col items-start">
    <span className="text-sm text-muted-foreground">{label}</span>
    <span className="text-2xl font-semibold">{value}</span>
  </div>
);

const SectionHeader: React.FC<{ eyebrow?: string; title: string; subtitle?: string }> = ({ eyebrow, title, subtitle }) => (
  <div className="max-w-2xl mx-auto text-center mb-10">
    {eyebrow && <p className="uppercase tracking-widest text-xs text-emerald-500 mb-2">{eyebrow}</p>}
    <h2 className="text-3xl md:text-4xl font-bold">{title}</h2>
    {subtitle && <p className="text-muted-foreground mt-3">{subtitle}</p>}
  </div>
);

const NavLink: React.FC<{ href: string; children: React.ReactNode }> = ({ href, children }) => (
  <a href={href} className="text-sm hover:text-emerald-500 transition-colors">
    {children}
  </a>
);

// ----- Main page component -----
function VerdaxWebsite() {
  const [email, setEmail] = useState("");
  const COLORS = useMemo(() => ["#059669", "#10b981", "#34d399", "#6ee7b7", "#a7f3d0"], []);

  function onSubscribe(e: React.FormEvent) {
    e.preventDefault();
    alert(`Thanks! We'll keep you posted at ${email}.`);
    setEmail("");
  }

  return (
    <div className="min-h-screen bg-gradient-to-b from-black via-zinc-950 to-black text-white">
      {/* Top Bar */}
      <header className="sticky top-0 z-50 backdrop-blur supports-[backdrop-filter]:bg-black/50 border-b border-white/10">
        <div className="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <div className="h-8 w-8 rounded-xl bg-emerald-500 flex items-center justify-center font-black">V</div>
            <span className="font-semibold tracking-wide">VERDAX</span>
            <Badge className="ml-2 bg-emerald-600/20 text-emerald-300 border border-emerald-700/50">Profit for the Planet</Badge>
          </div>
          <nav className="hidden md:flex items-center gap-6">
            <NavLink href="#about">About</NavLink>
            <NavLink href="#tokenomics">Tokenomics</NavLink>
            <NavLink href="#roadmap">Roadmap</NavLink>
            <NavLink href="#widgets">Buy / Stake</NavLink>
            <NavLink href="#news">Updates</NavLink>
            <NavLink href="#faq">FAQ</NavLink>
            <a href="#whitepaper" className="hidden md:inline-flex items-center gap-2 text-sm text-emerald-300">
              <Download className="h-4 w-4" />Whitepaper
            </a>
          </nav>
          <div className="flex items-center gap-2">
            <Button className="rounded-2xl" onClick={() => document.getElementById("widgets")?.scrollIntoView({ behavior: "smooth" })}>
              Buy VDX
            </Button>
          </div>
        </div>
      </header>

      {/* Hero */}
      <section className="relative overflow-hidden">
        <div className="absolute inset-0 bg-[radial-gradient(ellipse_at_top,_var(--tw-gradient-stops))] from-emerald-600/20 via-transparent to-transparent" />
        <div className="max-w-7xl mx-auto px-4 py-20 md:py-28 grid md:grid-cols-2 gap-10 items-center">
          <motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.6 }}>
            <h1 className="text-4xl md:text-6xl font-extrabold leading-tight">
              Mineral-Backed Crypto for <span className="text-emerald-400">Sustainable Wealth</span>
            </h1>
            <p className="mt-5 text-lg text-muted-foreground">
              Verdax (VDX) bridges Africa’s resources with the global economy. Own a stake in a transparent, impact-driven digital asset.
            </p>
            <div className="mt-8 flex flex-wrap gap-3">
              <Button size="lg" className="rounded-2xl" onClick={() => document.getElementById("widgets")?.scrollIntoView({ behavior: "smooth" })}>
                <span className="flex items-center gap-2">Buy VDX <ArrowRight className="h-4 w-4" /></span>
              </Button>
              <Button size="lg" variant="secondary" className="rounded-2xl" asChild>
                <a href="#whitepaper" className="flex items-center gap-2"><Download className="h-4 w-4" /> Whitepaper</a>
              </Button>
            </div>
            <div className="mt-8 grid grid-cols-2 sm:grid-cols-4 gap-6">
              <Stat label="Total Supply" value="1,000,000,000 VDX" />
              <Stat label="Backed by" value="Audited Minerals" />
              <Stat label="Chain" value="EVM (tbd)" />
              <Stat label="Ticker" value="VDX" />
            </div>
          </motion.div>

          <motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.8 }}>
            <Card className="bg-zinc-900/60 border-white/10">
              <CardHeader>
                <CardTitle className="flex items-center gap-2"><LineChart className="h-5 w-5 text-emerald-400" /> Live Price & Market</CardTitle>
              </CardHeader>
              <CardContent>
                {/* Replace with your real price widget / TradingView / CoinGecko embed */}
                <div className="aspect-video w-full rounded-xl bg-black/60 border border-white/10 flex items-center justify-center text-sm text-muted-foreground">
                  Live price widget placeholder
                </div>
                <div className="mt-4 text-xs text-muted-foreground">
                  *This is a placeholder. Integrate your preferred widget or API chart.
                </div>
              </CardContent>
            </Card>
          </motion.div>
        </div>
      </section>

      {/* About */}
      <section id="about" className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <SectionHeader
            eyebrow="About"
            title="A transparent bridge between real resources and Web3"
            subtitle="Verdax democratizes access to African mineral wealth while funding environmental and community impact."
          />
          <div className="grid md:grid-cols-4 gap-6">
            {benefits.map((b, i) => (
              <Card key={i} className="bg-zinc-900/60 border-white/10">
                <CardContent className="p-6">
                  <b.icon className="h-6 w-6 text-emerald-400" />
                  <h3 className="mt-4 font-semibold">{b.title}</h3>
                  <p className="mt-2 text-sm text-muted-foreground">{b.desc}</p>
                </CardContent>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* Tokenomics */}
      <section id="tokenomics" className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <SectionHeader
            eyebrow="Tokenomics"
            title="Fair, transparent distribution"
            subtitle="Hard-capped at 1,000,000,000 VDX with long-term alignment through vesting and community funds."
          />
          <div className="grid md:grid-cols-2 gap-10 items-center">
            <div className="h-80">
              <ResponsiveContainer width="100%" height="100%">
                <PieChart>
                  <Pie data={tokenomics} dataKey="value" nameKey="name" innerRadius={60} outerRadius={100} paddingAngle={1}>
                    {tokenomics.map((_, index) => (
                      <Cell key={index} fill={["#059669", "#10b981", "#34d399", "#6ee7b7", "#a7f3d0"][index % 5]} />
                    ))}
                  </Pie>
                  <Tooltip contentStyle={{ backgroundColor: "#0a0a0a", border: "1px solid #222", borderRadius: 12 }} />
                  <Legend />
                </PieChart>
              </ResponsiveContainer>
            </div>
            <div className="space-y-4">
              {tokenomics.map((t, i) => (
                <div key={i} className="flex items-center gap-3">
                  <div className="h-3 w-3 rounded-full" style={{ background: ["#059669", "#10b981", "#34d399", "#6ee7b7", "#a7f3d0"][i % 5] }} />
                  <p className="text-sm"><span className="font-medium">{t.name}:</span> {t.value}%</p>
                </div>
              ))}
              <div className="pt-4 text-sm text-muted-foreground">
                Team allocation subject to multi-year vesting & cliffs. Community & Sustainability fund governed on-chain.
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* Roadmap */}
      <section id="roadmap" className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <SectionHeader
            eyebrow="Roadmap"
            title="From launch to lasting impact"
            subtitle="Milestones that balance growth, compliance, and community outcomes."
          />
          <div className="grid md:grid-cols-3 gap-6">
            {roadmap.map((r, idx) => (
              <Card key={idx} className="bg-zinc-900/60 border-white/10">
                <CardHeader>
                  <CardTitle className="flex items-center gap-2">
                    {idx === 0 && <Sparkles className="h-5 w-5 text-emerald-400" />}
                    {idx === 1 && <Building2 className="h-5 w-5 text-emerald-400" />}
                    {idx === 2 && <Users2 className="h-5 w-5 text-emerald-400" />}
                    {r.phase}
                  </CardTitle>
                </CardHeader>
                <CardContent className="space-y-2">
                  {r.items.map((it, i) => (
                    <div key={i} className="flex items-start gap-2 text-sm">
                      <CheckCircle2 className="mt-0.5 h-4 w-4 text-emerald-400" />
                      <span className="text-muted-foreground">{it}</span>
                    </div>
                  ))}
                </CardContent>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* Widgets: Swap / Staking / Governance */}
      <section id="widgets" className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <SectionHeader
            eyebrow="Access"
            title="Buy, stake, and govern Verdax"
            subtitle="Use the integrated widgets to participate in the Verdax economy. Replace the placeholders below with your providers."
          />
          <Tabs defaultValue="swap" className="w-full">
            <TabsList className="grid grid-cols-3 w-full sm:w-auto">
              <TabsTrigger value="swap">Buy / Swap</TabsTrigger>
              <TabsTrigger value="stake">Stake</TabsTrigger>
              <TabsTrigger value="govern">Govern</TabsTrigger>
            </TabsList>

            <TabsContent value="swap" className="mt-6">
              <Card className="bg-zinc-900/60 border-white/10">
                <CardHeader>
                  <CardTitle className="flex items-center gap-2"><Wallet className="h-5 w-5 text-emerald-400" /> Buy / Swap VDX</CardTitle>
                </CardHeader>
                <CardContent>
                  {/* Embed your preferred swap widget (e.g., Uniswap/0x/Rango). Update src URL. */}
                  <iframe
                    title="swap-widget"
                    src="https://example.com/swap-widget?token=VDX"
                    className="w-full h-[560px] rounded-xl border border-white/10"
                  />
                  <p className="mt-3 text-xs text-muted-foreground">
                    Replace the iframe src with your provider’s URL. Ensure correct VDX token contract address and chain.
                  </p>
                </CardContent>
              </Card>
            </TabsContent>

            <TabsContent value="stake" className="mt-6">
              <Card className="bg-zinc-900/60 border-white/10">
                <CardHeader>
                  <CardTitle className="flex items-center gap-2"><ShieldCheck className="h-5 w-5 text-emerald-400" /> Staking Portal</CardTitle>
                </CardHeader>
                <CardContent>
                  <iframe
                    title="staking-portal"
                    src="https://example.com/staking"
                    className="w-full h-[560px] rounded-xl border border-white/10"
                  />
                  <p className="mt-3 text-xs text-muted-foreground">
                    Connect contracts and UI for lock periods, APY, and rewards claim.
                  </p>
                </CardContent>
              </Card>
            </TabsContent>

            <TabsContent value="govern" className="mt-6">
              <Card className="bg-zinc-900/60 border-white/10">
                <CardHeader>
                  <CardTitle className="flex items-center gap-2"><Globe2 className="h-5 w-5 text-emerald-400" /> Governance</CardTitle>
                </CardHeader>
                <CardContent>
                  <div className="aspect-video w-full rounded-xl bg-black/60 border border-white/10 flex items-center justify-center text-sm text-muted-foreground">
                    Governance portal placeholder
                  </div>
                </CardContent>
              </Card>
            </TabsContent>
          </Tabs>

          <div className="mt-6 text-xs text-muted-foreground">
            *Participation may be restricted in some jurisdictions. Perform your own due diligence and comply with local regulations.
          </div>
        </div>
      </section>

      {/* Whitepaper & Docs */}
      <section id="whitepaper" className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <SectionHeader
            eyebrow="Docs"
            title="Whitepaper & Technical Documentation"
            subtitle="Dive into token design, reserve attestations, and ecosystem architecture."
          />
          <div className="grid md:grid-cols-3 gap-6">
            <Card className="bg-zinc-900/60 border-white/10">
              <CardHeader>
                <CardTitle>Whitepaper</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-sm text-muted-foreground">Comprehensive overview of Verdax mission, economics, and governance.</p>
                <Button className="mt-4 w-full rounded-2xl" asChild>
                  <a href="https://your-whitepaper-link.pdf" target="_blank" rel="noreferrer">
                    <Download className="mr-2 h-4 w-4" /> Download PDF
                  </a>
                </Button>
              </CardContent>
            </Card>

            <Card className="bg-zinc-900/60 border-white/10">
              <CardHeader>
                <CardTitle>Smart Contract Audit</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-sm text-muted-foreground">External security review with findings and remediations.</p>
                <Button variant="secondary" className="mt-4 w-full rounded-2xl" asChild>
                  <a href="#">View Report (soon)</a>
                </Button>
              </CardContent>
            </Card>

            <Card className="bg-zinc-900/60 border-white/10">
              <CardHeader>
                <CardTitle>Reserve Attestations</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-sm text-muted-foreground">Independent verification of mineral reserves and custody.</p>
                <Button variant="secondary" className="mt-4 w-full rounded-2xl" asChild>
                  <a href="#">View Attestations (soon)</a>
                </Button>
              </CardContent>
            </Card>
          </div>
        </div>
      </section>

      {/* News / Updates */}
      <section id="news" className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <SectionHeader
            eyebrow="Updates"
            title="Latest announcements & partnerships"
            subtitle="Post news, AMAs, listing notes, and community proposals here."
          />
          <div className="grid md:grid-cols-3 gap-6">
            {[1, 2, 3].map((n) => (
              <Card key={n} className="bg-zinc-900/60 border-white/10">
                <CardHeader>
                  <CardTitle>Announcement {n}</CardTitle>
                </CardHeader>
                <CardContent>
                  <p className="text-sm text-muted-foreground">Short summary of the update. Replace with CMS or markdown content.</p>
                  <Button variant="link" className="px-0" asChild>
                    <a href="#">
                      Read more <ArrowRight className="h-4 w-4 ml-1" />
                    </a>
                  </Button>
                </CardContent>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* Newsletter */}
      <section className="py-20 border-t border-white/10">
        <div className="max-w-7xl mx-auto px-4">
          <div className="relative overflow-hidden rounded-3xl border border-white/10 bg-gradient-to-r from-emerald-700/20 via-emerald-600/10 to-transparent p-8 md:p-12">
            <div className="max-w-2xl">
              <h3 className="text-2xl md:text-3xl font-bold">Join the Verdax community</h3>
              <p className="mt-2 text-sm text-emerald-200/90">Get presale dates, listings, and impact reports in your inbox.</p>
              <form onSubmit={onSubscribe} className="mt-6 flex flex-col sm:flex-row gap-3">
                <Input
                  type="email"
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  placeholder="Enter your email"
                  required
                  className="bg-zinc-900/60 border-white/10"
                />
                <Button type="submit" className="rounded-2xl">Subscribe</Button>
              </form>
              <p className="mt-2 text-xs text-muted-foreground">
                By subscribing you agree to our <a className="underline" href="#">Privacy Policy</a>.
              </p>
            </div>
          </div>
        </div>
      </section>

      {/* FAQ */}
      <section id="faq" className="py-20 border-t border-white/10">
        <div className="max-w-4xl mx-auto px-4">
          <SectionHeader eyebrow="FAQ" title="Answers to common questions" />
          <Accordion type="single" collapsible className="w-full">
            {faqs.map((f, i) => (
              <AccordionItem key={i} value={`item-${i}`}>
                <AccordionTrigger className="text-left">{f.q}</AccordionTrigger>
                <AccordionContent className="text-sm text-muted-foreground">{f.a}</AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </section>

      {/* Footer */}
      <footer className="border-t border-white/10 py-10">
        <div className="max-w-7xl mx-auto px-4 grid md:grid-cols-4 gap-8">
          <div>
            <div className="flex items-center gap-2">
              <div className="h-8 w-8 rounded-xl bg-emerald-500 flex items-center justify-center font-black">V</div>
              <span className="font-semibold tracking-wide">VERDAX</span>
            </div>
            <p className="mt-3 text-sm text-muted-foreground">
              Mineral-backed crypto for sustainable wealth. Profit for the Planet.
            </p>
          </div>
          <div>
            <h4 className="font-semibold mb-3">Project</h4>
            <ul className="space-y-2 text-sm">
              <li><a className="hover:text-emerald-400" href="#about">About</a></li>
              <li><a className="hover:text-emerald-400" href="#tokenomics">Tokenomics</a></li>
              <li><a className="hover:text-emerald-400" href="#roadmap">Roadmap</a></li>
              <li><a className="hover:text-emerald-400" href="#whitepaper">Whitepaper</a></li>
            </ul>
          </div>
          <div>
            <h4 className="font-semibold mb-3">Participate</h4>
            <ul className="space-y-2 text-sm">
              <li><a className="hover:text-emerald-400" href="#widgets">Buy / Swap</a></li>
              <li><a className="hover:text-emerald-400" href="#widgets">Stake</a></li>
              <li><a className="hover:text-emerald-400" href="#widgets">Govern</a></li>
            </ul>
          </div>
          <div>
            <h4 className="font-semibold mb-3">Connect</h4>
            <ul className="space-y-2 text-sm">
              <li>
                <a className="inline-flex items-center gap-2 hover:text-emerald-400" href="https://twitter.com/" target="_blank" rel="noreferrer">
                  <LinkIcon className="h-4 w-4" /> X (Twitter)
                </a>
              </li>
              <li>
                <a className="inline-flex items-center gap-2 hover:text-emerald-400" href="https://t.me/" target="_blank" rel="noreferrer">
                  <LinkIcon className="h-4 w-4" /> Telegram
                </a>
              </li>
              <li>
                <a className="inline-flex items-center gap-2 hover:text-emerald-400" href="https://discord.gg/" target="_blank" rel="noreferrer">
                  <LinkIcon className="h-4 w-4" /> Discord
                </a>
              </li>
              <li>
                <a className="inline-flex items-center gap-2 hover:text-emerald-400" href="mailto:team@verdax.org">
                  <LinkIcon className="h-4 w-4" /> team@verdax.org
                </a>
              </li>
            </ul>
          </div>
        </div>
        <div className="max-w-7xl mx-auto px-4 mt-8 text-xs text-muted-foreground">
          © {new Date().getFullYear()} Verdax Foundation. All rights reserved. This site is for informational purposes only
          and does not constitute financial advice or an offer to sell securities.
        </div>
      </footer>
    </div>
  );
}

// Next.js App Router page export
export default function Page() {
  return <VerdaxWebsite />;
}
