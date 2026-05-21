import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { Building2, ExternalLink, Search, MapPin, Users, Briefcase, TrendingUp } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import DataSourceBadge from '../components/DataSourceBadge.vue';
import BedrijfDetailModal, { type Bedrijf } from '../components/BedrijfDetailModal.vue';
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
} from 'recharts';

const bedrijven: Bedrijf[] = [
  { id: 1, naam: 'Accenture Almere', type: 'Consultancy', stad: 'Almere', vacatures: 34, medewerkers: 420, groei: 22, techStack: ['React', 'Java', 'AWS'], opgericht: 2008 },
  { id: 2, naam: 'Capgemini', type: 'IT Services', stad: 'Lelystad', vacatures: 28, medewerkers: 310, groei: 15, techStack: ['Python', 'Azure', 'Node.js'], opgericht: 2001 },
  { id: 3, naam: 'Sogeti', type: 'IT Services', stad: 'Almere', vacatures: 24, medewerkers: 280, groei: 18, techStack: ['Java', 'Docker', 'TypeScript'], opgericht: 2005 },
  { id: 4, naam: 'Gemeente Almere', type: 'Overheid', stad: 'Almere', vacatures: 19, medewerkers: 3200, groei: 8, techStack: ['Vue.js', 'PHP', 'PostgreSQL'], opgericht: 1984 },
  { id: 5, naam: 'Wehkamp', type: 'E-commerce', stad: 'Almere', vacatures: 17, medewerkers: 1100, groei: 31, techStack: ['React', 'Kotlin', 'Kubernetes'], opgericht: 1952 },
  { id: 6, naam: 'Flevoland ICT', type: 'IT Services', stad: 'Emmeloord', vacatures: 14, medewerkers: 95, groei: 12, techStack: ['PHP', 'MySQL', 'Vue.js'], opgericht: 2012 },
  { id: 7, naam: 'Provincie Flevoland', type: 'Overheid', stad: 'Lelystad', vacatures: 11, medewerkers: 850, groei: 5, techStack: ['Angular', 'Java', 'Oracle'], opgericht: 1986 },
  { id: 8, naam: 'Lelystad Airport', type: 'Transport', stad: 'Lelystad', vacatures: 9, medewerkers: 240, groei: 44, techStack: ['Python', 'React', 'AWS'], opgericht: 1990 },
  { id: 9, naam: 'Dronten Digital', type: 'IT Services', stad: 'Dronten', vacatures: 7, medewerkers: 42, groei: 28, techStack: ['Node.js', 'React', 'MongoDB'], opgericht: 2018 },
  { id: 10, naam: 'Zeewolde Tech', type: 'Startup', stad: 'Zeewolde', vacatures: 6, medewerkers: 18, groei: 67, techStack: ['TypeScript', 'GraphQL', 'Docker'], opgericht: 2021 },
];

const typeColors: Record<string, string> = {
  Consultancy: 'bg-sky-100 text-sky-700',
  'IT Services': 'bg-violet-100 text-violet-700',
  Overheid: 'bg-amber-100 text-amber-700',
  'E-commerce': 'bg-emerald-100 text-emerald-700',
  Transport: 'bg-rose-100 text-rose-700',
  Startup: 'bg-teal-100 text-teal-700',
};

const chartData = bedrijven.slice(0, 8).map((b) => ({ naam: b.naam.split(' ')[0], vacatures: b.vacatures, groei: b.groei }));

const Bedrijven: React.FC = () => {
  const [search, setSearch] = useState('');
  const [typeFilter, setTypeFilter] = useState('all');
  const [selected, setSelected] = useState<Bedrijf | null>(null);

  const types = ['all', ...Array.from(new Set(bedrijven.map((b) => b.type)))];

  const filtered = bedrijven.filter((b) => {
    const matchSearch =
      b.naam.toLowerCase().includes(search.toLowerCase()) ||
      b.stad.toLowerCase().includes(search.toLowerCase());
    const matchType = typeFilter === 'all' || b.type === typeFilter;
    return matchSearch && matchType;
  });

  return (
    <DashboardLayout title="Bedrijven" subtitle="Overzicht van werkgevers in Flevoland">
      <div className="space-y-6 max-w-[1400px]">
        <DataSourceBadge source="KvK & UWV" lastUpdated="15 jan 2026, 08:00" status="live" />

        <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
          {[
            { label: 'Actieve werkgevers', value: '149', icon: Building2 },
            { label: 'Totale vacatures', value: '608', icon: Briefcase },
            { label: 'Gem. groei per bedrijf', value: '+22%', icon: TrendingUp },
            { label: 'Nieuwe bedrijven 2025', value: '14', icon: Users },
          ].map((item, i) => (
            <motion.div
              key={item.label}
              initial={{ opacity: 0, y: 12 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ delay: i * 0.07 }}
              className="bg-white border border-slate-200 rounded-xl p-5"
            >
              <p className="text-2xl font-bold font-heading text-slate-800">{item.value}</p>
              <p className="text-xs text-slate-500 mt-1">{item.label}</p>
            </motion.div>
          ))}
        </div>

        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Vacatures per werkgever</h2>
          <p className="text-xs text-slate-500 mb-5">Top 8 werkgevers op vacatureaantal</p>
          <ResponsiveContainer width="100%" height={220}>
            <BarChart data={chartData} margin={{ top: 4, right: 4, left: -20, bottom: 0 }}>
              <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
              <XAxis dataKey="naam" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <YAxis tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <Tooltip contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }} />
              <Bar dataKey="vacatures" fill="#0ea5e9" radius={[4, 4, 0, 0]} maxBarSize={36} name="Vacatures" />
            </BarChart>
          </ResponsiveContainer>
        </div>

        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <div className="flex flex-col sm:flex-row gap-3 mb-5">
            <div className="relative flex-1">
              <Search size={15} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
              <input
                type="search"
                placeholder="Zoek bedrijf of stad..."
                value={search}
                onChange={(e) => setSearch(e.target.value)}
                className="w-full pl-9 pr-4 py-2 text-sm bg-slate-50 border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
              />
            </div>
            <select
              value={typeFilter}
              onChange={(e) => setTypeFilter(e.target.value)}
              className="text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 text-slate-700"
            >
              {types.map((t) => (
                <option key={t} value={t}>{t === 'all' ? 'Alle types' : t}</option>
              ))}
            </select>
          </div>

          <div className="space-y-3">
            {filtered.map((b, i) => (
              <motion.button
                key={b.id}
                initial={{ opacity: 0, x: -10 }}
                animate={{ opacity: 1, x: 0 }}
                transition={{ delay: i * 0.04 }}
                onClick={() => setSelected(b)}
                className="w-full text-left flex items-center gap-4 p-4 rounded-lg border border-slate-100 hover:border-sky-200 hover:bg-sky-50/30 transition-all duration-200 group focus:outline-none focus:ring-2 focus:ring-sky-500/40"
              >
                <div className="w-10 h-10 rounded-lg bg-gradient-to-br from-slate-100 to-slate-200 flex items-center justify-center text-sm font-bold text-slate-600 shrink-0">
                  {b.naam.charAt(0)}
                </div>
                <div className="flex-1 min-w-0">
                  <div className="flex items-center gap-2 flex-wrap">
                    <p className="text-sm font-semibold text-slate-800">{b.naam}</p>
                    <span className={`text-[10px] px-2 py-0.5 rounded-full font-semibold ${typeColors[b.type] ?? 'bg-slate-100 text-slate-600'}`}>
                      {b.type}
                    </span>
                  </div>
                  <div className="flex items-center gap-3 mt-1 flex-wrap">
                    <span className="flex items-center gap-1 text-[10px] text-slate-400">
                      <MapPin size={10} /> {b.stad}
                    </span>
                    <span className="flex items-center gap-1 text-[10px] text-slate-400">
                      <Users size={10} /> {b.medewerkers} medewerkers
                    </span>
                    <div className="flex gap-1 flex-wrap">
                      {b.techStack.map((t) => (
                        <span key={t} className="text-[10px] px-1.5 py-0.5 bg-slate-100 text-slate-600 rounded font-mono">{t}</span>
                      ))}
                    </div>
                  </div>
                </div>
                <div className="text-right shrink-0">
                  <p className="text-sm font-bold text-slate-800">{b.vacatures} vacatures</p>
                  <p className="text-[10px] text-emerald-600 font-semibold">+{b.groei}% groei</p>
                </div>
                <ExternalLink size={14} className="text-slate-300 group-hover:text-sky-500 transition-colors duration-200 shrink-0" />
              </motion.button>
            ))}
          </div>

          {filtered.length === 0 && (
            <div className="text-center py-12">
              <Building2 size={28} className="text-slate-300 mx-auto mb-3" />
              <p className="text-slate-500 text-sm">Geen bedrijven gevonden</p>
            </div>
          )}
        </div>
      </div>

      <BedrijfDetailModal bedrijf={selected} onClose={() => setSelected(null)} />
    </DashboardLayout>
  );
};

export default Bedrijven;