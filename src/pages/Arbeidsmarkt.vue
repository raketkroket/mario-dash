import React from 'react';
import { motion } from 'framer-motion';
import { TrendingUp, TrendingDown, AlertTriangle, CheckCircle } from 'lucide-react';
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
  LineChart,
  Line,
  ReferenceLine,
} from 'recharts';

const vraagAanbodData = [
  { type: 'Frontend', vraag: 96, aanbod: 72, tekort: 24 },
  { type: 'Backend', vraag: 115, aanbod: 68, tekort: 47 },
  { type: 'API', vraag: 65, aanbod: 45, tekort: 20 },
  { type: 'Full-stack', vraag: 88, aanbod: 54, tekort: 34 },
  { type: 'DevOps', vraag: 52, aanbod: 38, tekort: 14 },
  { type: 'Mobile', vraag: 41, aanbod: 29, tekort: 12 },
];

const historischData = [
  { jaar: '2020', tekort: 42 },
  { jaar: '2021', tekort: 68 },
  { jaar: '2022', tekort: 95 },
  { jaar: '2023', tekort: 128 },
  { jaar: '2024', tekort: 162 },
  { jaar: '2025', tekort: 196 },
];

const knelpunten = [
  { rol: 'Senior Backend Developer', ratio: 2.1, ernst: 'kritiek' },
  { rol: 'API Integration Specialist', ratio: 1.8, ernst: 'hoog' },
  { rol: 'Full-stack Developer', rol2: '', ratio: 1.6, ernst: 'hoog' },
  { rol: 'Frontend Lead', ratio: 1.4, ernst: 'gemiddeld' },
  { rol: 'Junior Frontend Developer', ratio: 1.1, ernst: 'laag' },
];

const ernstColors: Record<string, string> = {
  kritiek: 'bg-rose-100 text-rose-700 border-rose-200',
  hoog: 'bg-amber-100 text-amber-700 border-amber-200',
  gemiddeld: 'bg-yellow-100 text-yellow-700 border-yellow-200',
  laag: 'bg-emerald-100 text-emerald-700 border-emerald-200',
};

const Arbeidsmarkt: React.FC = () => (
  <DashboardLayout title="Arbeidsmarkt" subtitle="Vraag en aanbod van Software Developers in Flevoland">
    <div className="space-y-6 max-w-[1400px]">
      <DataSourceBadge source="UWV & CBS" lastUpdated="15 jan 2026, 08:00" status="live" />

      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        {[
          { label: 'Totale vraag', value: '608', sub: 'Openstaande vacatures', trend: 'up', color: 'sky' },
          { label: 'Totaal aanbod', value: '412', sub: 'Actieve werkzoekenden', trend: 'down', color: 'violet' },
          { label: 'Tekort', value: '196', sub: 'Onvervulde posities', trend: 'up', color: 'rose' },
          { label: 'Vraag/aanbod ratio', value: '1.48×', sub: 'Markt in voordeel werkgever', trend: 'neutral', color: 'amber' },
        ].map((item, i) => (
          <motion.div
            key={item.label}
            initial={{ opacity: 0, y: 12 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ delay: i * 0.07 }}
            className="bg-white border border-slate-200 rounded-xl p-5"
          >
            <div className="flex items-start justify-between mb-3">
              <p className="text-xs text-slate-500">{item.label}</p>
              {item.trend === 'up' && <TrendingUp size={14} className="text-emerald-500" />}
              {item.trend === 'down' && <TrendingDown size={14} className="text-rose-500" />}
            </div>
            <p className="text-2xl font-bold font-heading text-slate-800">{item.value}</p>
            <p className="text-[10px] text-slate-400 mt-1">{item.sub}</p>
          </motion.div>
        ))}
      </div>

      <div className="grid grid-cols-1 xl:grid-cols-2 gap-6">
        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Vraag vs. Aanbod per type</h2>
          <p className="text-xs text-slate-500 mb-5">Vergelijking openstaande vacatures en beschikbare developers</p>
          <ResponsiveContainer width="100%" height={280}>
            <BarChart data={vraagAanbodData} margin={{ top: 4, right: 4, left: -20, bottom: 0 }}>
              <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
              <XAxis dataKey="type" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <YAxis tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <Tooltip contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }} />
              <Legend wrapperStyle={{ fontSize: '12px', paddingTop: '12px' }} />
              <Bar dataKey="vraag" fill="#0ea5e9" radius={[4, 4, 0, 0]} maxBarSize={28} name="Vraag" />
              <Bar dataKey="aanbod" fill="#8b5cf6" radius={[4, 4, 0, 0]} maxBarSize={28} name="Aanbod" />
              <Bar dataKey="tekort" fill="#f43f5e" radius={[4, 4, 0, 0]} maxBarSize={28} name="Tekort" />
            </BarChart>
          </ResponsiveContainer>
        </div>

        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Groei van het tekort (2020–2025)</h2>
          <p className="text-xs text-slate-500 mb-5">Cumulatief tekort aan developers in Flevoland</p>
          <ResponsiveContainer width="100%" height={280}>
            <LineChart data={historischData} margin={{ top: 4, right: 4, left: -20, bottom: 0 }}>
              <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
              <XAxis dataKey="jaar" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <YAxis tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <Tooltip contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }} />
              <ReferenceLine y={100} stroke="#f59e0b" strokeDasharray="4 4" label={{ value: 'Kritieke grens', fontSize: 10, fill: '#f59e0b' }} />
              <Line type="monotone" dataKey="tekort" stroke="#f43f5e" strokeWidth={2.5} dot={{ r: 4, fill: '#f43f5e' }} name="Tekort" />
            </LineChart>
          </ResponsiveContainer>
        </div>
      </div>

      <div className="bg-white border border-slate-200 rounded-xl p-6">
        <h2 className="font-heading font-bold text-slate-800 text-base mb-4">Knelpuntberoepen</h2>
        <div className="space-y-3">
          {knelpunten.map((k, i) => (
            <motion.div
              key={k.rol}
              initial={{ opacity: 0, x: -10 }}
              animate={{ opacity: 1, x: 0 }}
              transition={{ delay: i * 0.07 }}
              className="flex items-center gap-4 p-4 bg-slate-50 rounded-lg border border-slate-100"
            >
              <div className="flex items-center gap-2 flex-1 min-w-0">
                {k.ernst === 'kritiek' ? (
                  <AlertTriangle size={16} className="text-rose-500 shrink-0" />
                ) : (
                  <CheckCircle size={16} className="text-emerald-500 shrink-0" />
                )}
                <p className="text-sm font-semibold text-slate-800 truncate">{k.rol}</p>
              </div>
              <div className="flex items-center gap-3 shrink-0">
                <div className="text-right">
                  <p className="text-sm font-bold text-slate-800">{k.ratio}×</p>
                  <p className="text-[10px] text-slate-500">vraag/aanbod</p>
                </div>
                <span className={`text-[10px] px-2 py-0.5 rounded-full font-semibold border ${ernstColors[k.ernst]}`}>
                  {k.ernst}
                </span>
              </div>
            </motion.div>
          ))}
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        {[
          { titel: 'Instroom via onderwijs', value: '~180/jaar', sub: 'HBO/WO ICT-afgestudeerden in Flevoland', color: 'border-l-sky-500' },
          { titel: 'Uitstroom door pensionering', value: '~24/jaar', sub: 'Developers ouder dan 60 jaar', color: 'border-l-amber-500' },
          { titel: 'Netto tekortgroei', value: '+40/jaar', sub: 'Verwachte toename tekort per jaar', color: 'border-l-rose-500' },
        ].map((item) => (
          <div key={item.titel} className={`bg-white border border-slate-200 border-l-4 ${item.color} rounded-xl p-5`}>
            <p className="text-xs text-slate-500 mb-1">{item.titel}</p>
            <p className="text-xl font-bold font-heading text-slate-800">{item.value}</p>
            <p className="text-xs text-slate-400 mt-1">{item.sub}</p>
          </div>
        ))}
      </div>
    </div>
  </DashboardLayout>
);

export default Arbeidsmarkt;