import React, { useState } from 'react';
import { Filter, X } from 'lucide-react';

interface FilterBarProps {
  onFilterChange?: (filters: FilterState) => void;
}

export interface FilterState {
  regio: string;
  type: string;
  periode: string;
  niveau: string;
}

const FilterBar: React.FC<FilterBarProps> = ({ onFilterChange }) => {
  const [filters, setFilters] = useState<FilterState>({
    regio: 'all',
    type: 'all',
    periode: '2025',
    niveau: 'all',
  });
  const [showFilters, setShowFilters] = useState(false);

  const update = (key: keyof FilterState, value: string) => {
    const next = { ...filters, [key]: value };
    setFilters(next);
    onFilterChange?.(next);
  };

  const reset = () => {
    const def = { regio: 'all', type: 'all', periode: '2025', niveau: 'all' };
    setFilters(def);
    onFilterChange?.(def);
  };

  const hasActive = Object.entries(filters).some(([k, v]) => (k === 'periode' ? v !== '2025' : v !== 'all'));

  return (
    <div className="bg-white border border-slate-200 rounded-xl p-4 mb-6">
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-2">
          <Filter size={16} className="text-sky-600" />
          <span className="text-sm font-semibold text-slate-700">Filters</span>
          {hasActive && (
            <span className="px-2 py-0.5 bg-sky-100 text-sky-700 text-xs rounded-full font-medium">
              Actief
            </span>
          )}
        </div>
        <div className="flex items-center gap-2">
          {hasActive && (
            <button
              onClick={reset}
              className="flex items-center gap-1 text-xs text-slate-500 hover:text-red-500 transition-colors duration-200"
            >
              <X size={12} />
              Reset
            </button>
          )}
          <button
            onClick={() => setShowFilters(!showFilters)}
            className="text-xs text-sky-600 hover:text-sky-700 font-medium transition-colors duration-200 sm:hidden"
          >
            {showFilters ? 'Verbergen' : 'Tonen'}
          </button>
        </div>
      </div>

      <div className={`grid grid-cols-2 md:grid-cols-4 gap-3 mt-4 ${showFilters ? 'grid' : 'hidden sm:grid'}`}>
        <div>
          <label className="block text-xs text-slate-500 mb-1 font-medium">Regio</label>
          <select
            value={filters.regio}
            onChange={(e) => update('regio', e.target.value)}
            className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
          >
            <option value="all">Heel Flevoland</option>
            <option value="almere">Almere</option>
            <option value="lelystad">Lelystad</option>
            <option value="emmeloord">Emmeloord</option>
            <option value="dronten">Dronten</option>
            <option value="zeewolde">Zeewolde</option>
            <option value="urk">Urk</option>
          </select>
        </div>

        <div>
          <label className="block text-xs text-slate-500 mb-1 font-medium">Type Developer</label>
          <select
            value={filters.type}
            onChange={(e) => update('type', e.target.value)}
            className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
          >
            <option value="all">Alle types</option>
            <option value="frontend">Frontend Developer</option>
            <option value="backend">Backend Developer</option>
            <option value="api">API Developer</option>
            <option value="fullstack">Full-stack Developer</option>
          </select>
        </div>

        <div>
          <label className="block text-xs text-slate-500 mb-1 font-medium">Periode</label>
          <select
            value={filters.periode}
            onChange={(e) => update('periode', e.target.value)}
            className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
          >
            <option value="2025">2025</option>
            <option value="2024">2024</option>
            <option value="2023">2023</option>
            <option value="q4-2025">Q4 2025</option>
            <option value="q3-2025">Q3 2025</option>
          </select>
        </div>

        <div>
          <label className="block text-xs text-slate-500 mb-1 font-medium">Niveau</label>
          <select
            value={filters.niveau}
            onChange={(e) => update('niveau', e.target.value)}
            className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
          >
            <option value="all">Alle niveaus</option>
            <option value="junior">Junior (0–2 jr)</option>
            <option value="medior">Medior (2–5 jr)</option>
            <option value="senior">Senior (5+ jr)</option>
            <option value="lead">Lead / Principal</option>
          </select>
        </div>
      </div>
    </div>
  );
};

export default FilterBar;