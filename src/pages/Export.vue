import React, { useState } from 'react';
import { Download, FileText, Table, CheckCircle } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import { toast } from 'react-toastify';

const exportSets = [
  { id: 'vacatures', label: 'Vacatureoverzicht', beschrijving: 'Alle openstaande vacatures met details', records: 608 },
  { id: 'salaris', label: 'Salarisdata', beschrijving: 'Gemiddelde salarissen per type en niveau', records: 48 },
  { id: 'trends', label: 'Trenddata 2020–2025', beschrijving: 'Historische vacaturedata per jaar', records: 360 },
  { id: 'bedrijven', label: 'Werkgeversoverzicht', beschrijving: 'Top bedrijven met vacatureaantallen', records: 124 },
  { id: 'regio', label: 'Regiodata Flevoland', beschrijving: 'Werkgelegenheid per stad', records: 6 },
];

const Export: React.FC = () => {
  const [selected, setSelected] = useState<string[]>([]);
  const [format, setFormat] = useState<'csv' | 'pdf'>('csv');

  const toggle = (id: string) => {
    setSelected((prev) =>
      prev.includes(id) ? prev.filter((s) => s !== id) : [...prev, id]
    );
  };

  const handleExport = () => {
    if (selected.length === 0) {
      toast.error('Selecteer minimaal één dataset om te exporteren');
      return;
    }
    toast.success(`${selected.length} dataset(s) worden geëxporteerd als ${format.toUpperCase()}`);
  };

  return (
    <DashboardLayout title="Export" subtitle="Exporteer arbeidsmarktdata als CSV of PDF">
      <div className="space-y-6 max-w-3xl">
        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <h2 className="font-heading font-bold text-slate-800 text-base mb-4">Exportformaat</h2>
          <div className="flex gap-3">
            {(['csv', 'pdf'] as const).map((f) => (
              <button
                key={f}
                onClick={() => setFormat(f)}
                className={`flex items-center gap-2 px-5 py-3 rounded-lg border text-sm font-semibold transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40 ${
                  format === f
                    ? 'bg-sky-600 text-white border-sky-600'
                    : 'bg-white text-slate-700 border-slate-200 hover:bg-slate-50'
                }`}
              >
                {f === 'csv' ? <Table size={16} /> : <FileText size={16} />}
                {f.toUpperCase()}
              </button>
            ))}
          </div>
        </div>

        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <h2 className="font-heading font-bold text-slate-800 text-base mb-4">Selecteer datasets</h2>
          <div className="space-y-3">
            {exportSets.map((set) => {
              const isSelected = selected.includes(set.id);
              return (
                <button
                  key={set.id}
                  onClick={() => toggle(set.id)}
                  className={`w-full flex items-center gap-4 p-4 rounded-lg border text-left transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40 ${
                    isSelected
                      ? 'border-sky-300 bg-sky-50'
                      : 'border-slate-200 bg-white hover:bg-slate-50'
                  }`}
                >
                  <div
                    className={`w-5 h-5 rounded border-2 flex items-center justify-center shrink-0 transition-all duration-200 ${
                      isSelected ? 'bg-sky-600 border-sky-600' : 'border-slate-300'
                    }`}
                  >
                    {isSelected && <CheckCircle size={12} className="text-white" />}
                  </div>
                  <div className="flex-1 min-w-0">
                    <p className="text-sm font-semibold text-slate-800">{set.label}</p>
                    <p className="text-xs text-slate-500 mt-0.5">{set.beschrijving}</p>
                  </div>
                  <span className="text-xs text-slate-400 font-mono shrink-0">{set.records} records</span>
                </button>
              );
            })}
          </div>
        </div>

        <div className="flex items-center justify-between bg-white border border-slate-200 rounded-xl p-5">
          <div>
            <p className="text-sm font-semibold text-slate-800">
              {selected.length} dataset{selected.length !== 1 ? 's' : ''} geselecteerd
            </p>
            <p className="text-xs text-slate-500 mt-0.5">
              Exportformaat: <span className="font-semibold">{format.toUpperCase()}</span>
            </p>
          </div>
          <button
            onClick={handleExport}
            className="flex items-center gap-2 px-6 py-3 bg-sky-600 text-white text-sm font-semibold rounded-lg hover:bg-sky-700 hover:scale-105 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
          >
            <Download size={16} />
            Exporteren
          </button>
        </div>
      </div>
    </DashboardLayout>
  );
};

export default Export;