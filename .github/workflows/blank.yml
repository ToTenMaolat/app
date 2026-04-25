import React from 'react';
import { base44 } from '@/api/base44Client';
import { useQuery } from '@tanstack/react-query';
import { Radio, Rss, Eye, MonitorPlay } from 'lucide-react';
import StatCard from '../components/dashboard/StatCard';
import LiveStreamCard from '../components/dashboard/LiveStreamCard';
import ActivityChart from '../components/dashboard/ActivityChart';
import RecentActivity from '../components/dashboard/RecentActivity';

export default function Dashboard() {
  const { data: streams = [] } = useQuery({
    queryKey: ['streams'],
    queryFn: () => base44.entities.Stream.list('-created_date'),
  });

  const { data: feeds = [] } = useQuery({
    queryKey: ['feeds'],
    queryFn: () => base44.entities.RSSFeed.list('-created_date'),
  });

  const liveStreams = streams.filter(s => s.status === 'live');
  const totalViewers = streams.reduce((sum, s) => sum + (s.viewer_count || 0), 0);
  const activeFeeds = feeds.filter(f => f.status === 'active');

  return (
    <div className="space-y-6">
      {/* Header */}
      <div>
        <h1 className="text-2xl font-bold text-foreground">Dashboard</h1>
        <p className="text-sm text-muted-foreground mt-1">Overview of your streaming ecosystem</p>
      </div>

      {/* Stats */}
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <StatCard title="Total Streams" value={streams.length} icon={Radio} trend="+12% this week" trendUp />
        <StatCard title="Live Now" value={liveStreams.length} icon={MonitorPlay} accentClass="bg-red-500/15" />
        <StatCard title="Total Viewers" value={totalViewers.toLocaleString()} icon={Eye} trend="+8% avg" trendUp />
        <StatCard title="Active Feeds" value={activeFeeds.length} icon={Rss} accentClass="bg-accent/15" />
      </div>

      {/* Charts & Activity */}
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-4">
        <div className="lg:col-span-2">
          <ActivityChart />
        </div>
        <RecentActivity streams={streams} feeds={feeds} />
      </div>

      {/* Live Streams */}
      {liveStreams.length > 0 && (
        <div>
          <h2 className="text-lg font-semibold text-foreground mb-4 flex items-center gap-2">
            <span className="w-2 h-2 bg-red-500 rounded-full animate-pulse" />
            Live Now
          </h2>
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
            {liveStreams.map((stream, i) => (
              <LiveStreamCard key={stream.id} stream={stream} index={i} />
            ))}
          </div>
        </div>
      )}

      {/* Recent Streams */}
      <div>
        <h2 className="text-lg font-semibold text-foreground mb-4">Recent Streams</h2>
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
          {streams.slice(0, 8).map((stream, i) => (
            <LiveStreamCard key={stream.id} stream={stream} index={i} />
          ))}
        </div>
        {streams.length === 0 && (
          <div className="text-center py-16 bg-card border border-border rounded-xl">
            <Radio className="w-10 h-10 text-muted-foreground/30 mx-auto mb-3" />
            <p className="text-sm text-muted-foreground">No streams yet. Add your first stream to get started.</p>
          </div>
        )}
      </div>
    </div>
  );
}
