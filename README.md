18, engineer. I like to build.

send an email:cg077593@gmail.com ,[DM me on Twitter](https://x.com/PPlatypussss) Always happy to talk!



import React, { useState, useEffect } from 'react';

const GitHubContributionGraph = () => {
  const [contributions, setContributions] = useState({});
  const [hoveredCell, setHoveredCell] = useState(null);
  const [totalContributions, setTotalContributions] = useState(0);

  // Generate sample data for the past year
  useEffect(() => {
    const generateContributions = () => {
      const data = {};
      const today = new Date();
      const startDate = new Date(today);
      startDate.setFullYear(today.getFullYear() - 1);
      startDate.setDate(startDate.getDate() - startDate.getDay()); // Start from Sunday
      
      let total = 0;
      
      for (let d = new Date(startDate); d <= today; d.setDate(d.getDate() + 1)) {
        const dateStr = d.toISOString().split('T')[0];
        // Generate realistic contribution pattern
        const random = Math.random();
        const dayOfWeek = d.getDay();
        
        let count = 0;
        // Less activity on weekends
        const weekendMultiplier = (dayOfWeek === 0 || dayOfWeek === 6) ? 0.6 : 1;
        
        if (random > 0.65 * weekendMultiplier) {
          count = Math.floor(Math.random() * 8) + 1;
        }
        if (random > 0.85 * weekendMultiplier) {
          count = Math.floor(Math.random() * 15) + 8;
        }
        if (random > 0.95 * weekendMultiplier) {
          count = Math.floor(Math.random() * 25) + 15;
        }
        
        data[dateStr] = count;
        total += count;
      }
      
      setTotalContributions(total);
      return data;
    };

    setContributions(generateContributions());
  }, []);

  const getIntensityLevel = (count) => {
    if (count === 0) return 0;
    if (count <= 3) return 1;
    if (count <= 8) return 2;
    if (count <= 15) return 3;
    return 4;
  };

  const getIntensityColor = (level) => {
    const colors = [
      '#ebedf0', // No contributions (light gray)
      '#9be9a8', // Level 1 (light green)
      '#40c463', // Level 2 (medium green)
      '#30a14e', // Level 3 (dark green)
      '#216e39'  // Level 4 (darkest green)
    ];
    return colors[level];
  };

  const generateCalendarData = () => {
    const today = new Date();
    const startDate = new Date(today);
    startDate.setFullYear(today.getFullYear() - 1);
    
    // Find the first Sunday of the year or before
    const firstSunday = new Date(startDate);
    firstSunday.setDate(startDate.getDate() - startDate.getDay());
    
    const weeks = [];
    let currentWeek = [];
    
    for (let d = new Date(firstSunday); d <= today; d.setDate(d.getDate() + 1)) {
      const dateStr = d.toISOString().split('T')[0];
      const count = contributions[dateStr] || 0;
      
      currentWeek.push({
        date: dateStr,
        count: count,
        level: getIntensityLevel(count),
        dayOfWeek: d.getDay(),
        isCurrentYear: d >= startDate
      });
      
      if (d.getDay() === 6) { // Saturday
        weeks.push([...currentWeek]);
        currentWeek = [];
      }
    }
    
    // Add remaining days if any
    if (currentWeek.length > 0) {
      weeks.push(currentWeek);
    }
    
    return weeks;
  };

  const formatDate = (dateStr) => {
    const date = new Date(dateStr);
    const options = { 
      weekday: 'short', 
      year: 'numeric', 
      month: 'short', 
      day: 'numeric' 
    };
    return date.toLocaleDateString('en-US', options);
  };

  const getMonths = () => {
    const months = [];
    const today = new Date();
    
    for (let i = 11; i >= 0; i--) {
      const date = new Date(today.getFullYear(), today.getMonth() - i, 1);
      months.push({
        name: date.toLocaleDateString('en-US', { month: 'short' }),
        abbr: date.toLocaleDateString('en-US', { month: 'short' })
      });
    }
    
    return months;
  };

  const weeks = generateCalendarData();
  const months = getMonths();

  return (
    <div className="bg-white p-6 rounded-lg border border-gray-200 max-w-4xl mx-auto">
      {/* Profile Header */}
      <div className="flex items-center gap-4 mb-6">
        <div className="w-16 h-16 bg-gray-300 rounded-full flex items-center justify-center">
          <svg className="w-8 h-8 text-gray-600" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/>
          </svg>
        </div>
        <div>
          <h2 className="text-xl font-semibold text-gray-900">cgchiraggupta</h2>
          <p className="text-gray-600">Software Developer</p>
        </div>
      </div>

      {/* Stats */}
      <div className="mb-6">
        <div className="flex items-center gap-4 text-sm text-gray-600 mb-2">
          <span className="flex items-center gap-1">
            <svg className="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
              <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
            </svg>
            {totalContributions.toLocaleString()} contributions in 2025
          </span>
          <span>📁 24 Public Repos</span>
          <span>🕐 Joined GitHub 11 months ago</span>
        </div>
        <p className="text-xs text-gray-500">📧 venti.sillly@gmail.com</p>
      </div>

      {/* Contribution Graph */}
      <div className="mb-4">
        <h3 className="text-sm font-medium text-gray-700 mb-3">contributions in the last year</h3>
        
        <div className="relative">
          {/* Month labels */}
          <div className="flex ml-8 mb-1">
            {months.map((month, index) => (
              <div 
                key={index} 
                className="text-xs text-gray-500 flex-1 text-center"
                style={{ minWidth: '45px' }}
              >
                {index % 2 === 0 ? month.abbr : ''}
              </div>
            ))}
          </div>

          <div className="flex">
            {/* Day labels */}
            <div className="flex flex-col text-xs text-gray-500 mr-2 pt-2">
              <div className="h-3"></div>
              <div className="h-3 flex items-center">Mon</div>
              <div className="h-3"></div>
              <div className="h-3 flex items-center">Wed</div>
              <div className="h-3"></div>
              <div className="h-3 flex items-center">Fri</div>
              <div className="h-3"></div>
            </div>

            {/* Calendar grid */}
            <div className="flex gap-1 overflow-x-auto">
              {weeks.map((week, weekIndex) => (
                <div key={weekIndex} className="flex flex-col gap-1">
                  {Array.from({ length: 7 }, (_, dayIndex) => {
                    const day = week.find(d => d.dayOfWeek === dayIndex);
                    if (!day) {
                      return (
                        <div
                          key={dayIndex}
                          className="w-3 h-3 rounded-sm"
                        />
                      );
                    }

                    return (
                      <div
                        key={day.date}
                        className="w-3 h-3 rounded-sm cursor-pointer hover:ring-1 hover:ring-gray-400 transition-all duration-150"
                        style={{
                          backgroundColor: getIntensityColor(day.level),
                          opacity: day.isCurrentYear ? 1 : 0.6
                        }}
                        onMouseEnter={() => setHoveredCell(day)}
                        onMouseLeave={() => setHoveredCell(null)}
                        title={`${day.count} contributions on ${formatDate(day.date)}`}
                      />
                    );
                  })}
                </div>
              ))}
            </div>
          </div>

          {/* Tooltip */}
          {hoveredCell && (
            <div className="absolute bg-gray-800 text-white text-xs px-2 py-1 rounded shadow-lg pointer-events-none z-10 -mt-8 ml-4">
              <strong>{hoveredCell.count} contributions</strong> on {formatDate(hoveredCell.date)}
            </div>
          )}
        </div>

        {/* Legend */}
        <div className="flex items-center justify-between mt-3">
          <div className="text-xs text-gray-500">
            Learn how we count contributions
          </div>
          <div className="flex items-center gap-1 text-xs text-gray-500">
            <span>Less</span>
            <div className="flex gap-1">
              {[0, 1, 2, 3, 4].map(level => (
                <div
                  key={level}
                  className="w-3 h-3 rounded-sm"
                  style={{ backgroundColor: getIntensityColor(level) }}
                />
              ))}
            </div>
            <span>More</span>
          </div>
        </div>
      </div>

      {/* Footer */}
      <div className="text-xs text-gray-500 mt-4">
        Last updated on 4th June 2025 using magic ✨
      </div>
    </div>
  );
};

export default GitHubContributionGraph;
