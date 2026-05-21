import React, { useState, useMemo } from 'react';
import { motion } from 'framer-motion';
import { Briefcase, MapPin, Clock, ChevronRight, Search } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import FilterBar from '../components/FilterBar.vue';
import VacatureDetailModal, { type Vacature } from '../components/VacatureDetailModal.vue';

const vacatures: Vacature[] = [
  { id: 1, titel: 'Senior Frontend Developer', bedrijf: 'Accenture', stad: 'Almere', type: 'Frontend', niveau: 'Senior', salaris: '€70k–€85k', geplaatst: '2 dagen geleden', remote: true },
  { id: 2, titel: 'Backend Engineer (Node.js)', bedrijf: 'Capgemini', stad: 'Lelystad', type: 'Backend', niveau: 'Medior', salaris: '€55k–€68k', geplaatst: '3 dagen geleden', remote: false },
  { id: 3, titel: 'API Developer (REST/GraphQL)', bedrijf: 'Wehkamp', stad: 'Almere', type: 'API', niveau: 'Medior', salaris: '€52k–€65k', geplaatst: '1 dag geleden', remote: true },
  { id: 4, titel: 'Junior React Developer', bedrijf: 'Flevoland ICT', stad: 'Emmeloord', type: 'Frontend', niveau: 'Junior', salaris: '€35k–€42k', geplaatst: '5 dagen geleden', remote: false },
  { id: 5, titel: 'Full-stack Developer (Python/React)', bedrijf: 'Sogeti', stad: 'Almere', type: 'Backend', niveau: 'Senior', salaris: '€75k–€90k', geplaatst: '1 week geleden', remote: true },
  { id: 6, titel: 'API Integration Specialist', bedrijf: 'Gemeente Almere', stad: 'Almere', type: 'API', niveau: 'Medior', salaris: '€48k–€60k', geplaatst: '4 dagen geleden', remote: false },
  { id: 7, titel: 'Lead Frontend Architect', bedrijf: 'Accenture', stad: 'Almere', type: 'Frontend', niveau: 'Lead', salaris: '€90k–€110k', geplaatst: '6 dagen geleden', remote: true },
  { id: 8, titel: 'Java Backend Developer', bedrijf: 'Capgemini', stad: 'Lelystad', type: 'Backend', niveau: 'Senior', salaris: '€72k–€88k', geplaatst: '2 weken geleden', remote: false },
];

const typeColors: Record<string, string> = {
  Frontend: 'bg-sky-100 text-sky-700',
  Backend: 'bg-violet-100 text-violet-700',
  API: 'bg-emerald-100 text-emerald-700',
};

const niveauColors: Record<string, string> = {
  Junior: 'bg-amber-100 text-amber-700',
  Medior: 'bg-blue-100 text-blue-700',
  Senior: 'bg-violet-100 text-violet-700',
  Lead: 'bg-rose-100 text-rose-700',
};

const parseSalaris = (s: string): number => {
  const match = s.match(/€(\d+)k/);
  return match ? parseInt(match[1], 10) : 0;
};

const parseDagen = (s: string): number => {
  if (s.includes('dag')) {
    const n = parseInt(s, 10);
    return isNaN(n) ? 1 : n;
  }
  if (s.includes('week')) {
    const n = parseInt(s, 10);
    return (isNaN(n) ? 1 : n) * 7;
  }
  return 30;
};

const Vacatures: React.FC = () => {
  const [search, setSearch] = useState('');
  const [sort, setSort] = useState<'recent' | 'salaris' | 'relevant'>('recent');
  const [selected, setSelected] = useState<Vacature | null>(null);

  const filtered = useMemo(() => {
    const result = vacatures.filter(
      (v) =>
        v.titel.toLowerCase().includes(search.toLowerCase()) ||
        v.bedrijf.toLowerCase().includes(search.toLowerCase())
    );
    if (sort === 'salaris') {
      return [...result].sort((a, b) => parseSalaris(b.salaris) - parseSalaris(a.salaris));
    }
    if (sort === 'recent') {
      return [...result].sort((a, b) => parseDagen(a.geplaatst) - parseDagen(b.geplaatst));
    }
    return result;
  }, [search, sort]);

  return (
    <DashboardLayout title="Vacatures" subtitle="Openstaande developer vacatures in Flevoland">
      <div className="space-y-6 max-w-[1400px]">
        <FilterBar />

        <div className="relative">
          <Search size={16} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
          <input
            type="search"
            placeholder="Zoek op functietitel of bedrijf..."
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            className="w-full pl-10 pr-4 py-2.5 text-sm bg-white border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
          />
        </div>

        <div className="flex items-center justify-between">
          <p className="text-sm text-slate-500">
            <span className="font-semibold text-slate-800">{filtered.length}</span> vacatures gevonden
          </p>
          <select
            value={sort}
            onChange={(e) => setSort(e.target.value as 'recent' | 'salaris' | 'relevant')}
            className="text-sm border border-slate-200 rounded-lg px-3 py-1.5 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 text-slate-700"
          >
            <option value="recent">Meest recent</option>
            <option value="salaris">Hoogste salaris</option>
            <option value="relevant">Meest relevant</option>
          </select>
        </div>

        <div className="space-y-3">
          {filtered.map((v, i) => (
            <motion.button
              key={v.id}
              initial={{ opacity: 0, y: 12 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ delay: i * 0.05 }}
              onClick={() => setSelected(v)}
              className="w-full text-left bg-white border border-slate-200 rounded-xl p-5 hover:shadow-md hover:-translate-y-0.5 transition-all duration-300 group focus:outline-none focus:ring-2 focus:ring-sky-500/40"
            >
              <div className="flex items-start justify-between gap-4">
                <div className="flex items-start gap-4 flex-1 min-w-0">
                  <div className="w-10 h-10 rounded-lg bg-gradient-to-br from-slate-100 to-slate-200 flex items-center justify-center text-sm font-bold text-slate-600 shrink-0">
                    {v.bedrijf.charAt(0)}
                  </div>
                  <div className="flex-1 min-w-0">
                    <h3 className="font-semibold text-slate-800 text-sm group-hover:text-sky-600 transition-colors duration-200">
                      {v.titel}
                    </h3>
                    <p className="text-xs text-slate-500 mt-0.5">{v.bedrijf}</p>
                    <div className="flex flex-wrap items-center gap-2 mt-2">
                      <span className={`text-[10px] px-2 py-0.5 rounded-full font-semibold ${typeColors[v.type] ?? 'bg-slate-100 text-slate-600'}`}>
                        {v.type}
                      </span>
                      <span className={`text-[10px] px-2 py-0.5 rounded-full font-semibold ${niveauColors[v.niveau] ?? 'bg-slate-100 text-slate-600'}`}>
                        {v.niveau}
                      </span>
                      {v.remote && (
                        <span className="text-[10px] px-2 py-0.5 rounded-full font-semibold bg-teal-100 text-teal-700">
                          Remote
                        </span>
                      )}
                      <span className="flex items-center gap-1 text-[10px] text-slate-400">
                        <MapPin size={10} />
                        {v.stad}
                      </span>
                      <span className="flex items-center gap-1 text-[10px] text-slate-400">
                        <Clock size={10} />
                        {v.geplaatst}
                      </span>
                    </div>
                  </div>
                </div>
                <div className="flex items-center gap-3 shrink-0">
                  <div className="text-right hidden sm:block">
                    <p className="text-sm font-bold text-slate-800">{v.salaris}</p>
                    <p className="text-[10px] text-slate-400">per jaar</p>
                  </div>
                  <ChevronRight size={16} className="text-slate-300 group-hover:text-sky-500 transition-colors duration-200" />
                </div>
              </div>
            </motion.button>
          ))}
        </div>

        {filtered.length === 0 && (
          <div className="bg-white border border-slate-200 rounded-xl p-12 text-center">
            <Briefcase size={32} className="text-slate-300 mx-auto mb-3" />
            <p className="text-slate-500 font-medium">Geen vacatures gevonden</p>
            <p className="text-sm text-slate-400 mt-1">Pas je zoekopdracht of filters aan</p>
          </div>
        )}
      </div>

      <VacatureDetailModal vacature={selected} onClose={() => setSelected(null)} />
    </DashboardLayout>
  );
};

export default Vacatures;