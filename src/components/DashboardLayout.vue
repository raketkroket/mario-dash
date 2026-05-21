import React from 'react';
import Sidebar from './Sidebar.vue';
import TopBar from './TopBar.vue';

interface DashboardLayoutProps {
  children: React.ReactNode;
  title: string;
  subtitle?: string;
}

const DashboardLayout: React.FC<DashboardLayoutProps> = ({ children, title, subtitle }) => {
  return (
    <div className="flex min-h-screen bg-[hsl(220,20%,97%)]">
      <Sidebar />
      <div className="flex-1 flex flex-col min-w-0">
        <TopBar title={title} subtitle={subtitle} />
        <main className="flex-1 p-6 overflow-auto">
          {children}
        </main>
      </div>
    </div>
  );
};

export default DashboardLayout;