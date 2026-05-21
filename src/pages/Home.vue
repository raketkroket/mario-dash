import React from 'react';
import { Briefcase, TrendingUp, Euro, Users } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import StatCard from '../components/StatCard';
import FilterBar from '../components/FilterBar.vue';
import DataSourceBadge from '../components/DataSourceBadge.vue';
import VacatureChart from '../components/VacatureChart.vue';
import TechStackChart from '../components/TechStackChart';
import VraagAanbodChart from '../components/VraagAanbodChart.vue';
import SalarisChart from '../components/SalarisChart.vue';
import RegioHeatmap from '../components/RegioHeatmap.vue';
import TopBedrijven from '../components/TopBedrijven.vue';

const Home: React.FC = () => {
  return (
    <DashboardLayout
      title="Arbeidsmarkt Dashboard"
      subtitle="Software Developers in Flevoland — Overzicht 2025"
    >
      <div className="space-y-6 max-w-[1400px]">
        <DataSourceBadge
          source="UWV & CBS Open Data"
          lastUpdated="15 jan 2026, 08:00"
          status="live"
        />

        <FilterBar />

        <section aria-label="Kernstatistieken">
          <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
            <StatCard
              title="Openstaande vacatures"
              value="608"
              change={14}
              changeLabel="t.o.v. vorig kwartaal"
              icon={Briefcase}
              color="sky"
              delay={0}
            />
            <StatCard
              title="Gem. jaarsalaris"
              value="€62.400"
              change={7}
              changeLabel="Medior niveau"
              icon={Euro}
              color="emerald"
              delay={0.08}
            />
            <StatCard
              title="Vacaturegroei YoY"
              value="+23%"
              change={23}
              changeLabel="Jaar-op-jaar groei"
              icon={TrendingUp}
              color="violet"
              delay={0.16}
            />
            <StatCard
              title="Actieve werkzoekenden"
              value="412"
              change={-5}
              changeLabel="Tekort: 196 developers"
              icon={Users}
              color="amber"
              delay={0.24}
            />
          </div>
        </section>

        <section aria-label="Vacatureontwikkeling en technologieën">
          <div className="grid grid-cols-1 xl:grid-cols-2 gap-6">
            <VacatureChart />
            <TechStackChart />
          </div>
        </section>

        <section aria-label="Salaris en vraag-aanbod">
          <div className="grid grid-cols-1 xl:grid-cols-2 gap-6">
            <SalarisChart />
            <VraagAanbodChart />
          </div>
        </section>

        <section aria-label="Regio en bedrijven">
          <div className="grid grid-cols-1 xl:grid-cols-2 gap-6">
            <RegioHeatmap />
            <TopBedrijven />
          </div>
        </section>

        <section aria-label="Trends samenvatting">
          <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
            {[
              {
                label: 'Snelst groeiende rol',
                value: 'API Developer',
                sub: '+38% in 12 maanden',
                color: 'border-l-sky-500',
              },
              {
                label: 'Meest gevraagde taal',
                value: 'TypeScript',
                sub: '162 actieve vacatures',
                color: 'border-l-violet-500',
              },
              {
                label: 'Grootste tekort',
                value: 'Backend Senior',
                sub: 'Vraag 2.1× hoger dan aanbod',
                color: 'border-l-rose-500',
              },
            ].map((item) => (
              <div
                key={item.label}
                className={`bg-white border border-slate-200 border-l-4 ${item.color} rounded-xl p-5`}
              >
                <p className="text-xs text-slate-500 mb-1">{item.label}</p>
                <p className="font-heading font-bold text-slate-800 text-lg">{item.value}</p>
                <p className="text-xs text-slate-500 mt-1">{item.sub}</p>
              </div>
            ))}
          </div>
        </section>
      </div>
    </DashboardLayout>
  );
};

export default Home;