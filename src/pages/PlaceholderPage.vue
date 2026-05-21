import React from 'react';
import { Construction } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';

interface PlaceholderPageProps {
  title: string;
  subtitle?: string;
}

const PlaceholderPage: React.FC<PlaceholderPageProps> = ({ title, subtitle }) => (
  <DashboardLayout title={title} subtitle={subtitle}>
    <div className="flex flex-col items-center justify-center min-h-[60vh] text-center">
      <div className="w-16 h-16 rounded-2xl bg-sky-50 border border-sky-100 flex items-center justify-center mb-6">
        <Construction size={28} className="text-sky-500" />
      </div>
      <h2 className="font-heading font-bold text-slate-800 text-xl mb-2">Binnenkort beschikbaar</h2>
      <p className="text-slate-500 text-sm max-w-sm">
        Deze pagina is nog in ontwikkeling. Gebruik Meku om content voor deze pagina te genereren.
      </p>
      <p className="mt-4 text-xs text-slate-400 font-mono bg-slate-50 border border-slate-200 px-4 py-2 rounded-lg">
        Use Meku to generate content for this page
      </p>
    </div>
  </DashboardLayout>
);

export default PlaceholderPage;