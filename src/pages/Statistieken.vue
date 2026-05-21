import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { BarChart3, TrendingUp, Award, Clock } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import DataSourceBadge from '../components/DataSourceBadge.vue';
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
  Legend,
  AreaChart,
  Area,
  PieChart,
  Pie,
  Cell,
} from 'recharts';

const salarisNiveauData = [
  { niveau: 'Junior', min: 32000, max: 42000, gem: 37000 },
  { niveau: 'Medior', min: 48000, max: 68000, gem: 58000 },
  { niveau: 'Senior', min: 68000, max: 92000, gem: 80000 },
  { niveau: 'Lead', min: 85000, max: 115000, gem: 100000 },
];

const contractData = [
  { name: 'Vast contract', value: 54, color: '#0ea5e9' },
  { name: 'Freelance', value: 28, color: '#8b5cf6' },
  { name: 'Tijdelijk', value: 12, color: '#10b981' },
  { name: 'Stage', value: 6, color: '#f59e0b' },
];

const ervaringData = [
  { maand: 'Jan', Junior: 38, Medior: 52, Senior: 28 },
  { maand: 'Mrt', Junior: 42, Medior: 58, Senior: 31 },
  { maand: 'Mei', Junior: 48, Medior: 65, Senior: 35 },
  { maand: 'Jul', Junior: 44, Medior: 70, Senior: 38 },
  { maand: 'Sep', Junior: 52, Medior: 78, Senior: 42 },
  { maand: 'Nov', Junior: 56, Medior: 82, Senior: 46 },
];

const certData = [
  { cert: 'AWS Certified', vraag: 87 },
  { cert: 'Azure Fundamentals', vraag: 74 },
  { cert: 'Scrum Master', vraag: 68 },
  { cert: 'Google Cloud', vraag: 52 },
  { cert: 'Kubernetes CKA', vraag: 44 },
  { cert: 'Docker DCA', vraag: 38 },
];

const formatSalaris = (value: number) => `€${(value / 1000).toFixed(0)}k`;

const tabs = ['Salaris', 'Contracten', 'Ervaring', 'Certificaten'];

const Statistieken: React.FC = () => {
  const [activeTab, setActiveTab] = useState('Salaris');

  return (
    <DashboardLayout title="Statistieken" subtitle="Uitgebreide arbeidsmarktstatistieken voor Flevoland">
      <div className="space-y-6 max-w-[1400px]">
        <DataSourceBadge source="CBS & UWV Open Data" lastUpdated="15 jan 2026, 08:00" status="live" />

        <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
          {[
            { label: 'Gem. salaris medior', value: '€58.000', icon: BarChart3, delay: 0 },
            { label: 'Hoogste groei rol', value: 'API Dev +38%', icon: TrendingUp, delay: 0.07 },
            { label: 'Meest gevraagd cert', value: 'AWS Certified', icon: Award, delay: 0.14 },
            { label: 'Gem. time-to-hire', value: '42 dagen', icon: Clock, delay: 0.21 },
          ].map((item) => (
            <motion.div
              key={item.label}
              initial={{ opacity: 0, y: 12 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ delay: item.delay }}
              className="bg-white border border-slate-200 rounded-xl p-5"
            >
              <div className="w-8 h-8 rounded-lg bg-sky-50 flex items-center justify-center mb-3">
                <item.icon size={16} className="text-sky-600" />
              </div>
              <p className="text-lg font-bold font-heading text-slate-800">{item.value}</p>
              <p className="text-xs text-slate-500 mt-0.5">{item.label}</p>
            </motion.div>
          ))}
        </div>

        <div className="bg-white border border-slate-200 rounded-xl overflow-hidden">
          <div className="flex border-b border-slate-200">
            {tabs.map((tab) => (
              <button
                key={tab}
                onClick={() => setActiveTab(tab)}
                className={`flex-1 px-4 py-3 text-sm font-semibold transition-all duration-200 focus:outline-none ${
                  activeTab === tab
                    ? 'text-sky-600 border-b-2 border-sky-600 bg-sky-50/50'
                    : 'text-slate-500 hover:text-slate-700 hover:bg-slate-50'
                }`}
              >
                {tab}
              </button>
            ))}
          </div>

          <div className="p-6">
            {activeTab === 'Salaris' && (
              <div>
                <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Salarisbandbreedte per niveau</h2>
                <p className="text-xs text-slate-500 mb-5">Min, gemiddeld en max bruto jaarsalaris</p>
                <ResponsiveContainer width="100%" height={300}>
                  <BarChart data={salarisNiveauData} margin={{ top: 4, right: 4, left: -8, bottom: 0 }}>
                    <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
                    <XAxis dataKey="niveau" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                    <YAxis tickFormatter={formatSalaris} tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                    <Tooltip
                      formatter={(value: number) => [`€${value.toLocaleString('nl-NL')}`, '']}
                      contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }}
                    />
                    <Legend wrapperStyle={{ fontSize: '12px', paddingTop: '12px' }} />
                    <Bar dataKey="min" fill="#bae6fd" radius={[4, 4, 0, 0]} maxBarSize={32} name="Minimum" />
                    <Bar dataKey="gem" fill="#0ea5e9" radius={[4, 4, 0, 0]} maxBarSize={32} name="Gemiddeld" />
                    <Bar dataKey="max" fill="#0369a1" radius={[4, 4, 0, 0]} maxBarSize={32} name="Maximum" />
                  </BarChart>
                </ResponsiveContainer>
              </div>
            )}

            {activeTab === 'Contracten' && (
              <div>
                <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Contractvorm verdeling</h2>
                <p className="text-xs text-slate-500 mb-5">Percentage per contracttype in 2025</p>
                <div className="flex flex-col md:flex-row items-center gap-8">
                  <ResponsiveContainer width="100%" height={280}>
                    <PieChart>
                      <Pie
                        data={contractData}
                        cx="50%"
                        cy="50%"
                        innerRadius={70}
                        outerRadius={110}
                        paddingAngle={3}
                        dataKey="value"
                      >
                        {contractData.map((entry, index) => (
                          <Cell key={`cell-${index}`} fill={entry.color} />
                        ))}
                      </Pie>
                      <Tooltip
                        formatter={(value: number) => [`${value}%`, 'Aandeel']}
                        contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }}
                      />
                    </PieChart>
                  </ResponsiveContainer>
                  <div className="space-y-4 min-w-[180px]">
                    {contractData.map((item) => (
                      <div key={item.name} className="flex items-center gap-3">
                        <span className="w-3 h-3 rounded-full shrink-0" style={{ background: item.color }} />
                        <span className="text-sm text-slate-600 flex-1">{item.name}</span>
                        <span className="text-sm font-bold text-slate-800">{item.value}%</span>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            )}

            {activeTab === 'Ervaring' && (
              <div>
                <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Vacatures per ervaringsniveau</h2>
                <p className="text-xs text-slate-500 mb-5">Maandelijkse verdeling 2025</p>
                <ResponsiveContainer width="100%" height={300}>
                  <AreaChart data={ervaringData} margin={{ top: 4, right: 4, left: -20, bottom: 0 }}>
                    <defs>
                      <linearGradient id="gradJunior" x1="0" y1="0" x2="0" y2="1">
                        <stop offset="5%" stopColor="#f59e0b" stopOpacity={0.15} />
                        <stop offset="95%" stopColor="#f59e0b" stopOpacity={0} />
                      </linearGradient>
                      <linearGradient id="gradMedior" x1="0" y1="0" x2="0" y2="1">
                        <stop offset="5%" stopColor="#0ea5e9" stopOpacity={0.15} />
                        <stop offset="95%" stopColor="#0ea5e9" stopOpacity={0} />
                      </linearGradient>
                      <linearGradient id="gradSenior" x1="0" y1="0" x2="0" y2="1">
                        <stop offset="5%" stopColor="#8b5cf6" stopOpacity={0.15} />
                        <stop offset="95%" stopColor="#8b5cf6" stopOpacity={0} />
                      </linearGradient>
                    </defs>
                    <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
                    <XAxis dataKey="maand" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                    <YAxis tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                    <Tooltip contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }} />
                    <Legend wrapperStyle={{ fontSize: '12px', paddingTop: '12px' }} />
                    <Area type="monotone" dataKey="Junior" stroke="#f59e0b" strokeWidth={2} fill="url(#gradJunior)" dot={false} />
                    <Area type="monotone" dataKey="Medior" stroke="#0ea5e9" strokeWidth={2} fill="url(#gradMedior)" dot={false} />
                    <Area type="monotone" dataKey="Senior" stroke="#8b5cf6" strokeWidth={2} fill="url(#gradSenior)" dot={false} />
                  </AreaChart>
                </ResponsiveContainer>
              </div>
            )}

            {activeTab === 'Certificaten' && (
              <div>
                <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Meest gevraagde certificaten</h2>
                <p className="text-xs text-slate-500 mb-5">Aantal vacatures dat certificaat vereist</p>
                <ResponsiveContainer width="100%" height={300}>
                  <BarChart data={certData} layout="vertical" margin={{ top: 0, right: 16, left: 24, bottom: 0 }}>
                    <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" horizontal={false} />
                    <XAxis type="number" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                    <YAxis dataKey="cert" type="category" tick={{ fontSize: 11, fill: '#64748b' }} axisLine={false} tickLine={false} width={120} />
                    <Tooltip
                      contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }}
                      formatter={(value: number) => [`${value} vacatures`, 'Vraag']}
                    />
                    <Bar dataKey="vraag" fill="#0ea5e9" radius={[0, 4, 4, 0]} maxBarSize={20} name="Vacatures" />
                  </BarChart>
                </ResponsiveContainer>
              </div>
            )}
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          {[
            { label: 'Meest gevraagde taal', value: 'TypeScript', sub: '162 vacatures vermelden TypeScript', color: 'border-l-sky-500' },
            { label: 'Snelst groeiend framework', value: 'Next.js', sub: '+52% meer vacatures dan vorig jaar', color: 'border-l-violet-500' },
            { label: 'Populairste cloud', value: 'AWS', sub: 'Gevraagd in 87 vacatures in Flevoland', color: 'border-l-emerald-500' },
          ].map((item) => (
            <div key={item.label} className={`bg-white border border-slate-200 border-l-4 ${item.color} rounded-xl p-5`}>
              <p className="text-xs text-slate-500 mb-1">{item.label}</p>
              <p className="font-heading font-bold text-slate-800 text-lg">{item.value}</p>
              <p className="text-xs text-slate-400 mt-1">{item.sub}</p>
            </div>
          ))}
        </div>
      </div>
    </DashboardLayout>
  );
};

export default Statistieken;