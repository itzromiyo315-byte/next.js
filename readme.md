import React, { useMemo, useState } from "react"; import { motion } from "framer-motion"; import { ShieldCheck, UserPlus, ScanFace, Trophy, Coins, BadgeCheck, Gift, Activity, ArrowRight, Star, Sparkles, ClipboardCheck, TrendingUp, QrCode, Users, Medal, } from "lucide-react"; import { Card, CardContent, CardHeader, CardTitle, CardDescription } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { Label } from "@/components/ui/label"; import { Progress } from "@/components/ui/progress"; import { Badge } from "@/components/ui/badge"; import { Separator } from "@/components/ui/separator";

const TIERS = [ { name: "Bronze", min: 0, max: 1000, multiplier: "1x", accent: "From starting line" }, { name: "Silver", min: 1001, max: 2500, multiplier: "1.5x", accent: "Fast progression" }, { name: "Gold", min: 2501, max: 3000, multiplier: "2x", accent: "Strong loyalty" }, { name: "Platinum", min: 3001, max: 5000, multiplier: "3x", accent: "Premium member" }, ];

const REWARDS = [ { points: 100, label: "Earphone" }, { points: 250, label: "Temple Glass" }, { points: 500, label: "Mobile Cover" }, { points: 750, label: "Mobile Charger" }, { points: 1000, label: "₹100 Discount Coupon" }, { points: 1500, label: "Airbuds" }, { points: 2000, label: "Mini BT Speaker" }, { points: 2500, label: "₹250 DJ Booking Discount" }, { points: 5000, label: "Free Equipment Service" }, { points: 7500, label: "Movie Ticket" }, { points: 10000, label: "Free Membership" }, ];

const TASKS = [ { name: "Service booking", points: "10–1000", detail: "Mobile repairing or DJ booking auto-credited after bill verification" }, { name: "New customer add", points: "+50", detail: "Credit only when new customer reaches minimum spend threshold" }, { name: "YouTube subscribe", points: "+25", detail: "Auto verify with screenshot or channel connect" }, { name: "Like / Comment / Share", points: "+10 each", detail: "Social proof tasks can be approved by admin or bot" }, { name: "Instagram / Facebook follow", points: "+80", detail: "Social tasks can be bundled into one campaign credit" }, { name: "WhatsApp join", points: "+20", detail: "Group / channel join can be tracked through invite logs" }, { name: "Google review", points: "+30", detail: "Approval after review ID or screenshot check" }, ];

const BOOSTERS = [ { level: 1, target: 500, multiplier: "2x" }, { level: 2, target: 1000, multiplier: "3x" }, { level: 3, target: 1500, multiplier: "4x" }, ];

function tierForPoints(points: number) { return [...TIERS].reverse().find((tier) => points >= tier.min) ?? TIERS[0]; }

function nextTarget(points: number) { const nextTier = TIERS.find((tier) => points < tier.min); return nextTier?.min ?? 5000; }

export default function VmdGamificationWebsite() { const [memberName, setMemberName] = useState("Aman Kumar"); const [memberId, setMemberId] = useState("VMD-2048"); const [points, setPoints] = useState(680); const [vCoins, setVCoins] = useState(420); const [verified, setVerified] = useState(true); const [kycDone, setKycDone] = useState(true); const [serviceBill, setServiceBill] = useState(2500);

const tier = useMemo(() => tierForPoints(points), [points]); const target = useMemo(() => nextTarget(points), [points]); const progressValue = Math.min(100, Math.round((points / target) * 100)); const rankToNext = Math.max(0, target - points); const booster = useMemo( () => BOOSTERS.find((b) => points < b.target) ?? BOOSTERS[BOOSTERS.length - 1], [points] );

const canClaimReward = (rewardPoints: number) => points >= rewardPoints;

const addServicePoints = () => { const earned = Math.max(10, Math.round(serviceBill / 10)); const coinsEarned = Math.max(5, Math.round(serviceBill / 50)); setPoints((p) => p + earned); setVCoins((c) => c + coinsEarned); };

const collectSocialPack = () => { setPoints((p) => p + 95); setVCoins((c) => c + 20); };

const levelUpBoost = () => { const bonus = points >= booster.target ? 0 : 50; setPoints((p) => p + bonus + 25); setVCoins((c) => c + 10); };

return ( <div className="min-h-screen bg-gradient-to-b from-slate-950 via-slate-900 to-slate-950 text-white"> <div className="mx-auto max-w-7xl px-4 py-8 md:px-6 lg:px-8"> <motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.5 }} className="rounded-3xl border border-white/10 bg-white/5 p-6 shadow-2xl backdrop-blur" > <div className="grid gap-6 lg:grid-cols-[1.15fr_0.85fr]"> <div> <div className="mb-4 inline-flex items-center gap-2 rounded-full border border-cyan-400/30 bg-cyan-400/10 px-4 py-2 text-sm text-cyan-200"> <Sparkles className="h-4 w-4" /> VMD Loyalty Automation System </div> <h1 className="text-3xl font-bold tracking-tight md:text-5xl"> Member registration se le kar <span className="text-cyan-300">tier-up</span> aur <span className="text-fuchsia-300">V-Coin progress</span> tak ka complete journey ✨ </h1> <p className="mt-4 max-w-2xl text-sm leading-6 text-slate-300 md:text-base"> Yeh website PDF me diye gaye loyalty flow ko digital automation me convert karti hai: KYC/verification, member ID generation, task-based points, rewards exchange, booster challenge, aur rank tracking sab ek dashboard me. </p> <div className="mt-6 flex flex-wrap gap-3"> <Button className="rounded-2xl bg-cyan-400 px-5 py-6 text-slate-950 hover:bg-cyan-300"> Start Registration <ArrowRight className="ml-2 h-4 w-4" /> </Button> <Button variant="outline" className="rounded-2xl border-white/15 bg-white/5 px-5 py-6 text-white hover:bg-white/10"> Scan QR / Open Member ID <QrCode className="ml-2 h-4 w-4" /> </Button> </div> </div>

<Card className="rounded-3xl border-white/10 bg-slate-950/70 text-white shadow-xl">
          <CardHeader>
            <CardTitle className="flex items-center gap-2 text-white">
              <BadgeCheck className="h-5 w-5 text-emerald-400" />
              Live Member Status
            </CardTitle>
            <CardDescription className="text-slate-400">Auto-updating membership snapshot</CardDescription>
          </CardHeader>
          <CardContent className="space-y-4">
            <div className="grid grid-cols-2 gap-3">
              <div className="rounded-2xl border border-white/10 bg-white/5 p-4">
                <div className="text-xs text-slate-400">Member Name</div>
                <div className="mt-1 font-semibold">{memberName}</div>
              </div>
              <div className="rounded-2xl border border-white/10 bg-white/5 p-4">
                <div className="text-xs text-slate-400">Member ID</div>
                <div className="mt-1 font-semibold">{memberId}</div>
              </div>
            </div>
            <div className="rounded-2xl border border-white/10 bg-white/5 p-4">
              <div className="flex items-center justify-between">
                <span className="text-sm text-slate-300">Current Tier</span>
                <Badge className="rounded-full bg-cyan-400 text-slate-950 hover:bg-cyan-300">{tier.name}</Badge>
              </div>
              <div className="mt-2 text-2xl font-bold">{points} Points</div>
              <div className="mt-1 text-sm text-slate-400">{tier.accent} · {tier.multiplier} earning speed</div>
            </div>
            <div>
              <div className="mb-2 flex items-center justify-between text-sm text-slate-300">
                <span>Progress to next tier</span>
                <span>{rankToNext} points left</span>
              </div>
              <Progress value={progressValue} className="h-3 bg-white/10" />
            </div>
            <div className="grid grid-cols-2 gap-3">
              <div className="rounded-2xl border border-white/10 bg-white/5 p-4">
                <div className="flex items-center gap-2 text-sm text-slate-300"><Coins className="h-4 w-4 text-yellow-400" /> V-Coins</div>
                <div className="mt-2 text-2xl font-bold">{vCoins}</div>
              </div>
              <div className="rounded-2xl border border-white/10 bg-white/5 p-4">
                <div className="flex items-center gap-2 text-sm text-slate-300"><ShieldCheck className="h-4 w-4 text-emerald-400" /> Verification</div>
                <div className="mt-2 text-2xl font-bold">{verified ? "Approved" : "Pending"}</div>
              </div>
            </div>
          </CardContent>
        </Card>
      </div>
    </motion.div>

    <div className="mt-8 grid gap-6 lg:grid-cols-3">
      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><UserPlus className="h-5 w-5 text-cyan-600" /> Registration</CardTitle>
          <CardDescription>Member onboarding + KYC + unique ID</CardDescription>
        </CardHeader>
        <CardContent className="space-y-4">
          <div>
            <Label className="text-slate-700">Member Name</Label>
            <Input value={memberName} onChange={(e) => setMemberName(e.target.value)} className="mt-1 rounded-2xl" />
          </div>
          <div>
            <Label className="text-slate-700">Member ID</Label>
            <Input value={memberId} onChange={(e) => setMemberId(e.target.value)} className="mt-1 rounded-2xl" />
          </div>
          <div className="flex items-center justify-between rounded-2xl border bg-slate-50 px-4 py-3">
            <div>
              <div className="font-medium">KYC & Identity</div>
              <div className="text-sm text-slate-500">Aadhar / PAN / Voter ID check</div>
            </div>
            <Button
              variant="outline"
              className="rounded-2xl"
              onClick={() => setKycDone((v) => !v)}
            >
              {kycDone ? "Verified" : "Verify"}
            </Button>
          </div>
          <Button className="w-full rounded-2xl bg-slate-950 py-6 text-white hover:bg-slate-800">
            Generate Member ID <ClipboardCheck className="ml-2 h-4 w-4" />
          </Button>
        </CardContent>
      </Card>

      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><Activity className="h-5 w-5 text-fuchsia-600" /> Automation Rules</CardTitle>
          <CardDescription>Auto-credit based on event triggers</CardDescription>
        </CardHeader>
        <CardContent className="space-y-4 text-sm">
          <div className="rounded-2xl border bg-slate-50 p-4">
            <div className="font-semibold">Rule engine</div>
            <p className="mt-1 text-slate-600">Bill upload, QR scan, screenshot validation, and admin approval can credit points and V-Coins automatically.</p>
          </div>
          <div className="rounded-2xl border bg-slate-50 p-4">
            <div className="font-semibold">Level-up logic</div>
            <p className="mt-1 text-slate-600">Each tier unlocks higher earning speed, special gifts, and higher discount privileges.</p>
          </div>
          <div className="rounded-2xl border bg-slate-50 p-4">
            <div className="font-semibold">Smart alerts</div>
            <p className="mt-1 text-slate-600">Low points, reward eligibility, and challenge deadlines trigger WhatsApp / SMS / dashboard notifications.</p>
          </div>
        </CardContent>
      </Card>

      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><TrendingUp className="h-5 w-5 text-emerald-600" /> Quick Actions</CardTitle>
          <CardDescription>Simulate member journey</CardDescription>
        </CardHeader>
        <CardContent className="space-y-3">
          <div className="rounded-2xl border bg-slate-50 p-4">
            <Label className="text-slate-700">Service bill amount</Label>
            <Input type="number" value={serviceBill} onChange={(e) => setServiceBill(Number(e.target.value || 0))} className="mt-1 rounded-2xl" />
          </div>
          <Button onClick={addServicePoints} className="w-full rounded-2xl bg-emerald-600 py-6 text-white hover:bg-emerald-500">
            Add Service Points + V-Coins
          </Button>
          <Button onClick={collectSocialPack} variant="outline" className="w-full rounded-2xl py-6">
            Collect Social Task Pack
          </Button>
          <Button onClick={levelUpBoost} variant="secondary" className="w-full rounded-2xl py-6">
            Apply Boost Challenge
          </Button>
        </CardContent>
      </Card>
    </div>

    <div className="mt-8 grid gap-6 lg:grid-cols-2">
      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><Users className="h-5 w-5 text-cyan-600" /> Member Journey Timeline</CardTitle>
          <CardDescription>End-to-end flow from registration to rewards</CardDescription>
        </CardHeader>
        <CardContent>
          <div className="space-y-4">
            {[
              ["1. Registration", "Name, mobile, identity and join date collected"],
              ["2. Verification", "KYC approved, unique member ID generated"],
              ["3. Activity Tracking", "Service, social tasks, referrals auto-credit points"],
              ["4. Tier Upgrade", "Bronze → Silver → Gold → Platinum based on points"],
              ["5. Rewards Exchange", "Points redeemed for gifts, coupons, and discounts"],
              ["6. Booster Challenge", "15-day sprint to multiply points and unlock bonuses"],
            ].map(([title, desc], idx) => (
              <div key={title} className="flex gap-4">
                <div className="flex h-10 w-10 shrink-0 items-center justify-center rounded-full bg-slate-950 text-white">
                  {idx + 1}
                </div>
                <div className="rounded-2xl border bg-slate-50 p-4 flex-1">
                  <div className="font-semibold">{title}</div>
                  <div className="text-sm text-slate-600">{desc}</div>
                </div>
              </div>
            ))}
          </div>
        </CardContent>
      </Card>

      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><Trophy className="h-5 w-5 text-amber-500" /> Tiers, Rewards & Progress</CardTitle>
          <CardDescription>Live progression bar and unlock rules</CardDescription>
        </CardHeader>
        <CardContent className="space-y-5">
          <div className="grid gap-3 md:grid-cols-2">
            {TIERS.map((t) => (
              <div key={t.name} className={`rounded-2xl border p-4 ${tier.name === t.name ? "border-cyan-300 bg-cyan-50" : "bg-slate-50"}`}>
                <div className="flex items-center justify-between">
                  <div className="font-semibold">{t.name}</div>
                  <Badge variant="secondary" className="rounded-full">{t.multiplier}</Badge>
                </div>
                <div className="mt-1 text-sm text-slate-600">{t.min} - {t.max} points</div>
              </div>
            ))}
          </div>
          <Separator />
          <div>
            <div className="mb-2 flex items-center justify-between text-sm">
              <span>Current progress</span>
              <span>{progressValue}% to next tier</span>
            </div>
            <Progress value={progressValue} className="h-3" />
          </div>
          <div className="rounded-2xl border bg-slate-50 p-4">
            <div className="flex items-center gap-2 font-semibold"><Sparkles className="h-4 w-4 text-fuchsia-600" /> Boost Challenge</div>
            <p className="mt-1 text-sm text-slate-600">{booster.level} = {booster.target} target with {booster.multiplier} multiplier. 15-day sprint can be auto-tracked through activity logs.</p>
          </div>
        </CardContent>
      </Card>
    </div>

    <div className="mt-8 grid gap-6 lg:grid-cols-2">
      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><Gift className="h-5 w-5 text-rose-500" /> Rewards Exchange</CardTitle>
          <CardDescription>Redeem points with instant eligibility checks</CardDescription>
        </CardHeader>
        <CardContent>
          <div className="space-y-3">
            {REWARDS.map((reward) => (
              <div key={reward.points} className={`flex items-center justify-between rounded-2xl border p-4 ${canClaimReward(reward.points) ? "bg-emerald-50" : "bg-slate-50"}`}>
                <div>
                  <div className="font-semibold">{reward.label}</div>
                  <div className="text-sm text-slate-500">{reward.points.toLocaleString("en-IN")} points</div>
                </div>
                <Badge className={`rounded-full ${canClaimReward(reward.points) ? "bg-emerald-600 text-white" : "bg-slate-200 text-slate-700"}`}>
                  {canClaimReward(reward.points) ? "Unlock" : "Locked"}
                </Badge>
              </div>
            ))}
          </div>
        </CardContent>
      </Card>

      <Card className="rounded-3xl border-white/10 bg-white text-slate-950 shadow-xl">
        <CardHeader>
          <CardTitle className="flex items-center gap-2"><Medal className="h-5 w-5 text-violet-600" /> Admin Automation Panel</CardTitle>
          <CardDescription>Suggested backend modules for production build</CardDescription>
        </CardHeader>
        <CardContent className="space-y-3 text-sm text-slate-600">
          {[
            "Member database + unique ID generator",
            "KYC verification queue with manual approval",
            "Point ledger with immutable transaction history",
            "V-Coin wallet and redemption history",
            "Tier engine and level-up triggers",
            "Notification scheduler (WhatsApp / email / SMS)",
            "Campaigns, anniversary challenges, and leaderboard",
            "QR-based member card for fast scanning at counter",
          ].map((item) => (
            <div key={item} className="flex items-start gap-3 rounded-2xl border bg-slate-50 p-4">
              <Star className="mt-0.5 h-4 w-4 text-amber-500" />
              <span>{item}</span>
            </div>
          ))}
          <div className="rounded-2xl border border-dashed border-slate-300 bg-slate-50 p-4 text-slate-700">
            Backend suggestion: Next.js + PostgreSQL + Prisma + Auth + cron jobs + webhook-based point crediting.
          </div>
        </CardContent>
      </Card>
    </div>

    <div className="mt-8 rounded-3xl border border-cyan-400/20 bg-cyan-400/10 p-6 text-cyan-50">
      <div className="flex flex-col gap-3 md:flex-row md:items-center md:justify-between">
        <div>
          <h2 className="text-2xl font-bold">Ready for productization 🚀</h2>
          <p className="mt-1 max-w-3xl text-sm text-cyan-100/80">
            This prototype is structured as a conversion-focused loyalty app. It can be extended into a member portal, counter dashboard, and admin CRM.
          </p>
        </div>
        <div className="flex gap-3">
          <Button className="rounded-2xl bg-white text-slate-950 hover:bg-cyan-100">Export Member Card</Button>
          <Button variant="outline" className="rounded-2xl border-white/20 bg-transparent text-white hover:bg-white/10">Open Admin</Button>
        </div>
      </div>
    </div>
  </div>
</div>

); }packages/next/README.md