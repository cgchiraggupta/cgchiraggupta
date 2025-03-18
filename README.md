18, engineer. I like to build.

send an email:cg077593@gmail.com,[DM me on Twitter](https://x.com/PPlatypussss) Always happy to talk!

import { Card, CardContent } from "@/components/ui/card";
import { Star, GitCommit, GitPullRequest, GitMerge, Eye, AlertCircle, Coffee } from "lucide-react";

const GitHubStats = () => {
  return (
    <Card className="w-full max-w-md p-4 bg-gray-900 text-white rounded-xl shadow-lg">
      <CardContent className="flex flex-col gap-3">
        <h2 className="text-lg font-semibold">Chirag's GitHub Stats</h2>
        <Stat icon={<Star className="text-yellow-400" />} label="Total Stars Earned" value="204" />
        <Stat icon={<GitCommit className="text-green-400" />} label="Total Commits (2025)" value="490" />
        <Stat icon={<GitPullRequest className="text-blue-400" />} label="Total PRs" value="140" />
        <Stat icon={<GitMerge className="text-purple-400" />} label="Merged PRs" value="114" />
        <Stat icon={<Eye className="text-orange-400" />} label="PRs Reviewed" value="90" />
        <Stat icon={<AlertCircle className="text-red-400" />} label="Total Issues" value="88" />

        <a
          href="https://buymeacoffee.com/perryyy"
          target="_blank"
          rel="noopener noreferrer"
          className="flex items-center justify-center gap-2 px-4 py-2 mt-4 text-black bg-yellow-400 rounded-lg hover:bg-yellow-500 transition"
        >
          <Coffee className="w-5 h-5" />
          <span>Buy Me a Coffee</span>
        </a>
      </CardContent>
    </Card>
  );
};

const Stat = ({ icon, label, value }) => (
  <div className="flex items-center gap-3">
    {icon}
    <div>
      <p className="text-sm">{label}</p>
      <p className="text-lg font-bold">{value}</p>
    </div>
  </div>
);

export default GitHubStats;

![Profile Views](https://komarev.com/ghpvc/?username=cgchiraggupta&color=blue)
