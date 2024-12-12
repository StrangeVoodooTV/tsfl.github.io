# tsfl.github.io
The TS Fantasy Football Hub
const App = () => {
  const mountRef = useRef(null);
  useEffect(() => {
    // Scene setup
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ alpha: true });
    
    renderer.setSize(window.innerWidth, window.innerHeight);
    mountRef.current.appendChild(renderer.domElement);
    
    camera.position.z = 5;
    
    // Animation
    const animate = () => {
      requestAnimationFrame(animate);
      renderer.render(scene, camera);
    };
    
    animate();
    
    // Cleanup
    return () => {
      mountRef.current.removeChild(renderer.domElement);
      renderer.dispose();
    };
  }, []);
  return (
    <div style={{
      position: 'relative',
      width: '100vw',
      height: '100vh',
      backgroundColor: '#1a1a1a',
    }}>
      <div ref={mountRef} style={{
        position: 'absolute',
        top: 0,
        left: 0,
        width: '100%',
        height: '100%',
        zIndex: 1,
      }} />
      <div style={{
        position: 'absolute',
        top: 0,
        left: 0,
        width: '100%',
        height: '100%',
        zIndex: 2,
      }}>
        <nav style={{
          backgroundColor: '#2d2d2d',
          padding: '1rem',
          color: '#ffffff',
          borderBottom: '1px solid #404040',
        }}>
          <ul style={{
            display: 'flex',
            justifyContent: 'space-around',
            listStyle: 'none',
            margin: 0,
            padding: 0,
          }}>
            <li><a href="#" style={{ color: '#ffffff', textDecoration: 'none' }}>Home</a></li>
            <li>
              <TeamDropdown />
            </li>
            <li><a href="#" style={{ color: '#ffffff', textDecoration: 'none' }}>Statistics</a></li>
            <li><a href="#" style={{ color: '#ffffff', textDecoration: 'none' }}>Free Agency</a></li>
            <li><a href="#" style={{ color: '#ffffff', textDecoration: 'none' }}>Standings</a></li>
          </ul>
        </nav>
        <main style={{
          padding: '2rem',
          color: '#ffffff',
          height: 'calc(100% - 60px)',
          overflow: 'auto',
        }}>
          <div style={{
            backgroundColor: '#000000',
            borderRadius: '8px',
            padding: '1.5rem',
            marginBottom: '2rem',
            border: '2px solid #ff6b00',
            boxShadow: '0 0 10px rgba(255, 107, 0, 0.3)',
          }}>
            <h1 style={{ marginTop: 0, color: '#ffffff' }}>NFL Fantasy Football</h1>
            <div className="news-scroller" style={{
              height: '180px',
              overflow: 'hidden',
              position: 'relative',
              backgroundColor: '#353535',
              borderRadius: '4px',
              padding: '0.5rem',
            }}>
              <AutoScrollingNews />
            </div>
          </div>
          <div style={{
            display: 'grid',
            gridTemplateColumns: 'repeat(auto-fit, minmax(250px, 1fr))',
            gap: '1.5rem',
            maxWidth: '1400px',
            margin: '0 auto',
          }}>
            <QuickStats />
            <TopPerformers />
            <UpcomingGames />
            <NFLSchedule />
          </div>
        </main>
      </div>
    </div>
  );
};
// Auto-scrolling news component
const AutoScrollingNews = () => {
  const [newsIndex, setNewsIndex] = useState(0);
  const [newsItems, setNewsItems] = useState([]);
  useEffect(() => {
    const newsData = [
      {
        title: "Cardinals' Marvin Harrison Jr. leads rookies in TD catches, but lacks consistency",
        category: "NFL",
        description: "Our weekly roundup of first-year players looks at the No. 4 overall pick and why he hasn't made the huge splash many expected in Arizona.",
        link: "https://www.foxsports.com/stories/nfl/cardinals-marvin-harrison-jr-leads-rookies-td-catches-lacks-consistency"
      },
      {
        title: "2024 NFL Draft redo: Bears take a QB at No. 1 ... but not Caleb Williams",
        category: "NFL",
        description: "If given the opportunity to re-select the No. 1 pick, would the Bears still take Caleb Williams? We answer that in this redo of the top-10 picks of the 2024 NFL Draft.",
        link: "https://www.foxsports.com/stories/nfl/2024-nfl-draft-redo-bears-caleb-williams-jayden-daniels"
      },
      {
        title: "The Daily Ranker: Top 10 seasons by an NFL running back",
        category: "NFL",
        description: "The NFL has seen some special seasons, but these rushers put together seasons that truly represent greatness."
      },
      {
        title: "2024 NFL AFC, NFC Championship odds: Chiefs, Lions favored",
        category: "NFL",
        description: "Will the Chiefs represent the AFC in the Super Bowl once again? And who will emerge from an NFC full of contenders? See the latest AFC and NFC champion odds."
      },
      {
        title: "2024 NFL Playoff Scenarios: Which teams can clinch in Week 15?",
        category: "NFL",
        description: "Four more NFL teams can clinch a spot in the playoffs this weekend."
      }
    ];
    setNewsItems(newsData);
  }, []);
  useEffect(() => {
    const interval = setInterval(() => {
      setNewsIndex((prevIndex) => (prevIndex + 1) % newsItems.length);
    }, 10000);
    return () => clearInterval(interval);
  }, [newsItems.length]);
  if (newsItems.length === 0) {
    return <div>Loading news...</div>;
  }
  const handlePrevious = () => {
    setNewsIndex((prevIndex) => (prevIndex - 1 + newsItems.length) % newsItems.length);
  };
  const handleNext = () => {
    setNewsIndex((prevIndex) => (prevIndex + 1) % newsItems.length);
  };
  return (
    <div style={{
      transition: 'all 0.5s ease-in-out',
      padding: '0.5rem',
      height: '100%',
      position: 'relative',
    }}>
      <button
        onClick={handlePrevious}
        style={{
          position: 'absolute',
          left: '0.5rem',
          top: '50%',
          transform: 'translateY(-50%)',
          backgroundColor: '#404040',
          border: 'none',
          borderRadius: '50%',
          width: '32px',
          height: '32px',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          cursor: 'pointer',
          color: '#ffffff',
          zIndex: 3,
          opacity: '0.7',
          transition: 'opacity 0.3s ease',
        }}
        onMouseEnter={(e) => e.target.style.opacity = '1'}
        onMouseLeave={(e) => e.target.style.opacity = '0.7'}
      >
        ←
      </button>
      <div style={{
        backgroundColor: '#353535',
        padding: '1rem 2.5rem',
        borderRadius: '4px',
        height: '100%',
        display: 'flex',
        flexDirection: 'column',
        gap: '0.5rem',
      }}>
        <div style={{
          backgroundColor: '#404040',
          color: '#ffffff',
          padding: '0.25rem 0.5rem',
          borderRadius: '4px',
          display: 'inline-block',
          alignSelf: 'flex-start',
          fontSize: '0.8rem',
        }}>
          {newsItems[newsIndex].category}
        </div>
        <a 
          href={newsItems[newsIndex].link}
          target="_blank"
          rel="noopener noreferrer"
          style={{
            textDecoration: 'none',
            cursor: 'pointer',
            display: 'block',
          }}
        >
          <h3 style={{
            color: '#ffffff',
            margin: '0',
            fontSize: '1.1rem',
            fontWeight: '600',
            transition: 'color 0.3s ease',
          }}
          onMouseEnter={(e) => e.target.style.color = '#4a9eff'}
          onMouseLeave={(e) => e.target.style.color = '#ffffff'}
          >
            {newsItems[newsIndex].title}
          </h3>
          <p style={{
            color: '#b0b0b0',
            margin: '0.5rem 0 0 0',
            fontSize: '0.9rem',
            lineHeight: '1.4',
          }}>
            {newsItems[newsIndex].description}
          </p>
        </a>
      </div>
      <button
        onClick={handleNext}
        style={{
          position: 'absolute',
          right: '0.5rem',
          top: '50%',
          transform: 'translateY(-50%)',
          backgroundColor: '#404040',
          border: 'none',
          borderRadius: '50%',
          width: '32px',
          height: '32px',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          cursor: 'pointer',
          color: '#ffffff',
          zIndex: 3,
          opacity: '0.7',
          transition: 'opacity 0.3s ease',
        }}
        onMouseEnter={(e) => e.target.style.opacity = '1'}
        onMouseLeave={(e) => e.target.style.opacity = '0.7'}
      >
        →
      </button>
    </div>
  );
};
// Quick stats component
const QuickStats = () => {
  return (
    <div style={{
      backgroundColor: '#000000',
      padding: '1.5rem',
      borderRadius: '8px',
      border: '2px solid #ff6b00',
      boxShadow: '0 0 10px rgba(255, 107, 0, 0.3)',
    }}>
      <h2 style={{ margin: '0 0 1rem 0', color: '#ffffff' }}>Quick Stats</h2>
      <div style={{ color: '#e0e0e0' }}>
        <p>Total Teams: 8</p>
        <p>Active Players: 176</p>
        <p>Season Week: 1</p>
      </div>
    </div>
  );
};
// Top performers component
const TopPerformers = () => {
  return (
    <div style={{
      backgroundColor: '#000000',
      padding: '1.5rem',
      borderRadius: '8px',
      border: '2px solid #ff6b00',
      boxShadow: '0 0 10px rgba(255, 107, 0, 0.3)',
    }}>
      <h2 style={{ margin: '0 0 1rem 0', color: '#ffffff' }}>Top Performers</h2>
      <div style={{ color: '#e0e0e0' }}>
        <p>QB: TBD</p>
        <p>RB: TBD</p>
        <p>WR: TBD</p>
      </div>
    </div>
  );
};
// Upcoming games component
const UpcomingGames = () => {
  return (
    <div style={{
      backgroundColor: '#000000',
      padding: '1.5rem',
      borderRadius: '8px',
      border: '2px solid #ff6b00',
      boxShadow: '0 0 10px rgba(255, 107, 0, 0.3)',
    }}>
      <h2 style={{ margin: '0 0 1rem 0', color: '#ffffff' }}>Upcoming Games</h2>
      <div style={{ color: '#e0e0e0' }}>
        <p>Next matchup loading...</p>
      </div>
    </div>
  );
};
// Game Details Modal Component
const GameDetailsModal = ({ game, onClose }) => {
  if (!game) return null;
  return (
    <div style={{
      position: 'fixed',
      top: 0,
      left: 0,
      right: 0,
      bottom: 0,
      backgroundColor: 'rgba(0, 0, 0, 0)',
      backdropFilter: 'blur(5px)',
      display: 'flex',
      justifyContent: 'center',
      alignItems: 'center',
      zIndex: 1000,
      animation: 'fadeIn 0.3s ease forwards',
    }}>
      <style>
        {`
          @keyframes fadeIn {
            from { background-color: rgba(0, 0, 0, 0); }
            to { background-color: rgba(0, 0, 0, 0.75); }
          }
          @keyframes slideIn {
            from { transform: translateY(-20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
          }
        `}
      </style>
      <div style={{
        backgroundColor: '#2d2d2d',
        borderRadius: '8px',
        padding: '2rem',
        maxWidth: '600px',
        width: '90%',
        position: 'relative',
        maxHeight: '90vh',
        overflow: 'auto',
        animation: 'slideIn 0.3s ease forwards',
      }}>
        <button
          onClick={onClose}
          style={{
            position: 'absolute',
            top: '1rem',
            right: '1rem',
            backgroundColor: 'transparent',
            border: 'none',
            color: '#ffffff',
            fontSize: '1.5rem',
            cursor: 'pointer',
            padding: '0.5rem',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center',
            width: '32px',
            height: '32px',
            borderRadius: '50%',
            transition: 'background-color 0.3s ease',
          }}
          onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
          onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
        >
          ×
        </button>
        <div style={{ 
          display: 'flex', 
          alignItems: 'center', 
          justifyContent: 'center',
          gap: '1rem',
          marginBottom: '1.5rem'
        }}>
          <div style={{
            width: '48px',
            height: '48px',
            backgroundColor: '#404040',
            borderRadius: '50%',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center',
            fontSize: '0.9rem',
            fontWeight: 'bold',
            color: '#ffffff'
          }}>
            {game.teams.away.substring(0, 3).toUpperCase()}
          </div>
          <h2 style={{ color: '#ffffff', margin: 0 }}>@</h2>
          <div style={{
            width: '48px',
            height: '48px',
            backgroundColor: '#404040',
            borderRadius: '50%',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center',
            fontSize: '0.9rem',
            fontWeight: 'bold',
            color: '#ffffff'
          }}>
            {game.teams.home.substring(0, 3).toUpperCase()}
          </div>
        </div>
        <div style={{ color: '#e0e0e0', marginBottom: '1.5rem' }}>
          <p style={{ 
            fontSize: '1.1rem', 
            marginBottom: '1rem',
            textAlign: 'center',
            fontWeight: 'bold'
          }}>{game.date} - {game.time}</p>
          <div style={{ 
            backgroundColor: '#404040', 
            padding: '1rem', 
            borderRadius: '4px',
            marginBottom: '1rem'
          }}>
            <h3 style={{ color: '#ffffff', marginTop: 0 }}>Game Info</h3>
            <p>Location: {game.venue}</p>
            <p>Broadcast: {game.broadcast}</p>
            <p>Weather: {game.weather}</p>
          </div>
          <div style={{ 
            backgroundColor: '#404040', 
            padding: '1rem', 
            borderRadius: '4px',
            marginBottom: '1rem'
          }}>
            <h3 style={{ color: '#ffffff', marginTop: 0 }}>Team Comparison</h3>
            <div style={{ 
              display: 'grid', 
              gridTemplateColumns: '1fr auto 1fr',
              gap: '1.5rem',
              textAlign: 'center',
              marginBottom: '1.5rem'
            }}>
              <div>
                <h4 style={{ color: '#ffffff', marginBottom: '1rem', borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>
                  {game.teams.away}
                </h4>
                <div style={{ marginBottom: '1.5rem' }}>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Record:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{game.records.away}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Points/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{(Math.random() * 10 + 20).toFixed(1)}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Points Allowed/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{(Math.random() * 10 + 18).toFixed(1)}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Yards/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{Math.floor(Math.random() * 100 + 300)}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Yards Allowed/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{Math.floor(Math.random() * 100 + 280)}</span>
                  </p>
                </div>
                <div>
                  <p style={{ color: '#b0b0b0', fontSize: '0.8rem', marginBottom: '0.5rem' }}>Last 3 Games</p>
                  <div style={{ display: 'flex', gap: '0.5rem', justifyContent: 'center' }}>
                    {['W', 'L', 'W'].map((result, index) => (
                      <div
                        key={index}
                        style={{
                          width: '24px',
                          height: '24px',
                          backgroundColor: result === 'W' ? '#4daf7c' : '#ff4a4a',
                          borderRadius: '4px',
                          display: 'flex',
                          alignItems: 'center',
                          justifyContent: 'center',
                          color: '#ffffff',
                          fontSize: '0.8rem',
                          fontWeight: 'bold'
                        }}
                      >
                        {result}
                      </div>
                    ))}
                  </div>
                </div>
              </div>
              <div style={{ alignSelf: 'center', color: '#ffffff', fontSize: '1.2rem', fontWeight: 'bold' }}>VS</div>
              <div>
                <h4 style={{ color: '#ffffff', marginBottom: '1rem', borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>
                  {game.teams.home}
                </h4>
                <div style={{ marginBottom: '1.5rem' }}>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Record:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{game.records.home}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Points/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{(Math.random() * 10 + 20).toFixed(1)}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Points Allowed/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{(Math.random() * 10 + 18).toFixed(1)}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Yards/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{Math.floor(Math.random() * 100 + 300)}</span>
                  </p>
                  <p style={{ margin: '0.5rem 0', display: 'flex', justifyContent: 'space-between', fontSize: '0.9rem' }}>
                    <span style={{ color: '#b0b0b0' }}>Yards Allowed/Game:</span>
                    <span style={{ color: '#ffffff', fontWeight: 'bold' }}>{Math.floor(Math.random() * 100 + 280)}</span>
                  </p>
                </div>
                <div>
                  <p style={{ color: '#b0b0b0', fontSize: '0.8rem', marginBottom: '0.5rem' }}>Last 3 Games</p>
                  <div style={{ display: 'flex', gap: '0.5rem', justifyContent: 'center' }}>
                    {['L', 'W', 'W'].map((result, index) => (
                      <div
                        key={index}
                        style={{
                          width: '24px',
                          height: '24px',
                          backgroundColor: result === 'W' ? '#4daf7c' : '#ff4a4a',
                          borderRadius: '4px',
                          display: 'flex',
                          alignItems: 'center',
                          justifyContent: 'center',
                          color: '#ffffff',
                          fontSize: '0.8rem',
                          fontWeight: 'bold'
                        }}
                      >
                        {result}
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div style={{ 
            backgroundColor: '#404040', 
            padding: '1rem', 
            borderRadius: '4px' 
          }}>
            <h3 style={{ color: '#ffffff', marginTop: 0, marginBottom: '1rem' }}>Team Leaders</h3>
            <div style={{
              display: 'grid',
              gridTemplateColumns: '1fr 1fr',
              gap: '2rem',
              marginBottom: '2rem'
            }}>
              <div>
                <h4 style={{ color: '#ffffff', fontSize: '1rem', marginBottom: '1rem', 
                     borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>{game.teams.away}</h4>
                <div style={{ display: 'flex', flexDirection: 'column', gap: '1.5rem' }}>
                  {/* Passing Leaders */}
                  <div style={{ 
                    backgroundColor: '#353535', 
                    padding: '1rem', 
                    borderRadius: '4px',
                    borderLeft: '3px solid #4a9eff'
                  }}>
                    <h5 style={{ color: '#ffffff', margin: '0 0 1rem 0', fontSize: '0.9rem', 
                         display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                      <span>Passing Leaders</span>
                      <span style={{ color: '#4a9eff', fontSize: '0.75rem' }}>Season Stats</span>
                    </h5>
                    {[
                      { name: 'Justin Herbert', position: 'QB', team: 'LAC', 
                        rating: 95.2, yards: 3234, tds: 22, ints: 8,
                        completionPct: 67.8, yardsPerAttempt: 7.4 }
                    ].map((player, index) => (
                      <div key={index} style={{ marginBottom: index > 0 ? '1rem' : 0 }}>
                        <div style={{ 
                          display: 'flex', 
                          justifyContent: 'space-between', 
                          alignItems: 'center',
                          marginBottom: '0.5rem'
                        }}>
                          <span style={{ 
                            fontSize: '0.9rem', 
                            fontWeight: 'bold', 
                            color: '#ffffff' 
                          }}>{player.name}</span>
                          <span style={{ 
                            fontSize: '0.8rem', 
                            color: '#b0b0b0' 
                          }}>{player.position} • {player.team}</span>
                        </div>
                        <div style={{ 
                          display: 'grid', 
                          gridTemplateColumns: 'repeat(2, 1fr)', 
                          gap: '0.5rem',
                          fontSize: '0.85rem'
                        }}>
                          <div>Rating: {player.rating}</div>
                          <div>TDs/INTs: {player.tds}/{player.ints}</div>
                          <div>Yards: {player.yards}</div>
                          <div>Comp %: {player.completionPct}%</div>
                          <div>Y/A: {player.yardsPerAttempt}</div>
                        </div>
                      </div>
                    ))}
                  </div>
                  
                  {/* Rushing Leaders */}
                  <div style={{ 
                    backgroundColor: '#353535', 
                    padding: '1rem', 
                    borderRadius: '4px',
                    borderLeft: '3px solid #4daf7c'
                  }}>
                    <h5 style={{ color: '#ffffff', margin: '0 0 1rem 0', fontSize: '0.9rem', 
                         display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                      <span>Rushing Leaders</span>
                      <span style={{ color: '#4daf7c', fontSize: '0.75rem' }}>Season Stats</span>
                    </h5>
                    {[
                      { name: 'Austin Ekeler', position: 'RB', team: 'LAC',
                        yards: 987, tds: 11, carries: 198,
                        avgPerCarry: 5.0, yardsPerGame: 82.3 }
                    ].map((player, index) => (
                      <div key={index} style={{ marginBottom: index > 0 ? '1rem' : 0 }}>
                        <div style={{ 
                          display: 'flex', 
                          justifyContent: 'space-between', 
                          alignItems: 'center',
                          marginBottom: '0.5rem'
                        }}>
                          <span style={{ 
                            fontSize: '0.9rem', 
                            fontWeight: 'bold', 
                            color: '#ffffff' 
                          }}>{player.name}</span>
                          <span style={{ 
                            fontSize: '0.8rem', 
                            color: '#b0b0b0' 
                          }}>{player.position} • {player.team}</span>
                        </div>
                        <div style={{ 
                          display: 'grid', 
                          gridTemplateColumns: 'repeat(2, 1fr)', 
                          gap: '0.5rem',
                          fontSize: '0.85rem'
                        }}>
                          <div>Yards: {player.yards}</div>
                          <div>TDs: {player.tds}</div>
                          <div>Carries: {player.carries}</div>
                          <div>Avg: {player.avgPerCarry} YPC</div>
                          <div>Y/G: {player.yardsPerGame}</div>
                        </div>
                      </div>
                    ))}
                  </div>
                  
                  {/* Receiving Leaders */}
                  <div style={{ 
                    backgroundColor: '#353535', 
                    padding: '1rem', 
                    borderRadius: '4px',
                    borderLeft: '3px solid #e6c619'
                  }}>
                    <h5 style={{ color: '#ffffff', margin: '0 0 1rem 0', fontSize: '0.9rem', 
                         display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                      <span>Receiving Leaders</span>
                      <span style={{ color: '#e6c619', fontSize: '0.75rem' }}>Season Stats</span>
                    </h5>
                    {[
                      { name: 'Keenan Allen', position: 'WR', team: 'LAC',
                        receptions: 108, yards: 1332, tds: 7,
                        targets: 147, yardsPerGame: 95.1 }
                    ].map((player, index) => (
                      <div key={index} style={{ marginBottom: index > 0 ? '1rem' : 0 }}>
                        <div style={{ 
                          display: 'flex', 
                          justifyContent: 'space-between', 
                          alignItems: 'center',
                          marginBottom: '0.5rem'
                        }}>
                          <span style={{ 
                            fontSize: '0.9rem', 
                            fontWeight: 'bold', 
                            color: '#ffffff' 
                          }}>{player.name}</span>
                          <span style={{ 
                            fontSize: '0.8rem', 
                            color: '#b0b0b0' 
                          }}>{player.position} • {player.team}</span>
                        </div>
                        <div style={{ 
                          display: 'grid', 
                          gridTemplateColumns: 'repeat(2, 1fr)', 
                          gap: '0.5rem',
                          fontSize: '0.85rem'
                        }}>
                          <div>Rec: {player.receptions}</div>
                          <div>Yards: {player.yards}</div>
                          <div>TDs: {player.tds}</div>
                          <div>Targets: {player.targets}</div>
                          <div>Y/G: {player.yardsPerGame}</div>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
              <div>
                <h4 style={{ color: '#ffffff', fontSize: '1rem', marginBottom: '1rem', 
                     borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>{game.teams.home}</h4>
                <div style={{ display: 'flex', flexDirection: 'column', gap: '1.5rem' }}>
                  {/* Passing Leaders */}
                  <div style={{ 
                    backgroundColor: '#353535', 
                    padding: '1rem', 
                    borderRadius: '4px',
                    borderLeft: '3px solid #4a9eff'
                  }}>
                    <h5 style={{ color: '#ffffff', margin: '0 0 1rem 0', fontSize: '0.9rem', 
                         display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                      <span>Passing Leaders</span>
                      <span style={{ color: '#4a9eff', fontSize: '0.75rem' }}>Season Stats</span>
                    </h5>
                    {[
                      { name: 'Josh Allen', position: 'QB', team: 'BUF', 
                        rating: 91.2, yards: 3375, tds: 25, ints: 13,
                        completionPct: 65.2, yardsPerAttempt: 7.8 },
                      { name: 'Kyle Allen', position: 'QB', team: 'BUF',
                        rating: 82.5, yards: 425, tds: 3, ints: 2,
                        completionPct: 61.8, yardsPerAttempt: 6.9 }
                    ].map((player, index) => (
                      <div key={index} style={{ marginBottom: index > 0 ? '1rem' : 0 }}>
                        <div style={{ 
                          display: 'flex', 
                          justifyContent: 'space-between', 
                          alignItems: 'center',
                          marginBottom: '0.5rem'
                        }}>
                          <span style={{ 
                            fontSize: '0.9rem', 
                            fontWeight: 'bold', 
                            color: '#ffffff' 
                          }}>{player.name}</span>
                          <span style={{ 
                            fontSize: '0.8rem', 
                            color: '#b0b0b0' 
                          }}>{player.position} • {player.team}</span>
                        </div>
                        <div style={{ 
                          display: 'grid', 
                          gridTemplateColumns: 'repeat(2, 1fr)', 
                          gap: '0.5rem',
                          fontSize: '0.85rem'
                        }}>
                          <div>Rating: {player.rating}</div>
                          <div>TDs/INTs: {player.tds}/{player.ints}</div>
                          <div>Yards: {player.yards}</div>
                          <div>Comp %: {player.completionPct}%</div>
                          <div>Y/A: {player.yardsPerAttempt}</div>
                        </div>
                      </div>
                    ))}
                  </div>
                  {/* Rushing Leaders */}
                  <div style={{ 
                    backgroundColor: '#353535', 
                    padding: '1rem', 
                    borderRadius: '4px',
                    borderLeft: '3px solid #4daf7c'
                  }}>
                    <h5 style={{ color: '#ffffff', margin: '0 0 1rem 0', fontSize: '0.9rem', 
                         display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                      <span>Rushing Leaders</span>
                      <span style={{ color: '#4daf7c', fontSize: '0.75rem' }}>Season Stats</span>
                    </h5>
                    {[
                      { name: 'James Cook', position: 'RB', team: 'BUF',
                        yards: 789, tds: 4, carries: 172,
                        avgPerCarry: 4.6, yardsPerGame: 65.8 },
                      { name: 'Josh Allen', position: 'QB', team: 'BUF',
                        yards: 342, tds: 6, carries: 65,
                        avgPerCarry: 5.3, yardsPerGame: 28.5 },
                      { name: 'Latavius Murray', position: 'RB', team: 'BUF',
                        yards: 298, tds: 3, carries: 78,
                        avgPerCarry: 3.8, yardsPerGame: 24.8 }
                    ].map((player, index) => (
                      <div key={index} style={{ marginBottom: index > 0 ? '1rem' : 0 }}>
                        <div style={{ 
                          display: 'flex', 
                          justifyContent: 'space-between', 
                          alignItems: 'center',
                          marginBottom: '0.5rem'
                        }}>
                          <span style={{ 
                            fontSize: '0.9rem', 
                            fontWeight: 'bold', 
                            color: '#ffffff' 
                          }}>{player.name}</span>
                          <span style={{ 
                            fontSize: '0.8rem', 
                            color: '#b0b0b0' 
                          }}>{player.position} • {player.team}</span>
                        </div>
                        <div style={{ 
                          display: 'grid', 
                          gridTemplateColumns: 'repeat(2, 1fr)', 
                          gap: '0.5rem',
                          fontSize: '0.85rem'
                        }}>
                          <div>Yards: {player.yards}</div>
                          <div>TDs: {player.tds}</div>
                          <div>Carries: {player.carries}</div>
                          <div>Avg: {player.avgPerCarry} YPC</div>
                          <div>Y/G: {player.yardsPerGame}</div>
                        </div>
                      </div>
                    ))}
                  </div>
                  {/* Receiving Leaders */}
                  <div style={{ 
                    backgroundColor: '#353535', 
                    padding: '1rem', 
                    borderRadius: '4px',
                    borderLeft: '3px solid #e6c619'
                  }}>
                    <h5 style={{ color: '#ffffff', margin: '0 0 1rem 0', fontSize: '0.9rem', 
                         display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                      <span>Receiving Leaders</span>
                      <span style={{ color: '#e6c619', fontSize: '0.75rem' }}>Season Stats</span>
                    </h5>
                    {[
                      { name: 'Stefon Diggs', position: 'WR', team: 'BUF',
                        receptions: 87, yards: 1042, tds: 8,
                        targets: 132, yardsPerGame: 80.2 },
                      { name: 'Dalton Kincaid', position: 'TE', team: 'BUF',
                        receptions: 58, yards: 495, tds: 3,
                        targets: 74, yardsPerGame: 41.3 }
                    ].map((player, index) => (
                      <div key={index} style={{ marginBottom: index > 0 ? '1rem' : 0 }}>
                        <div style={{ 
                          display: 'flex', 
                          justifyContent: 'space-between', 
                          alignItems: 'center',
                          marginBottom: '0.5rem'
                        }}>
                          <span style={{ 
                            fontSize: '0.9rem', 
                            fontWeight: 'bold', 
                            color: '#ffffff' 
                          }}>{player.name}</span>
                          <span style={{ 
                            fontSize: '0.8rem', 
                            color: '#b0b0b0' 
                          }}>{player.position} • {player.team}</span>
                        </div>
                        <div style={{ 
                          display: 'grid', 
                          gridTemplateColumns: 'repeat(2, 1fr)', 
                          gap: '0.5rem',
                          fontSize: '0.85rem'
                        }}>
                          <div>Rec: {player.receptions}</div>
                          <div>Yards: {player.yards}</div>
                          <div>TDs: {player.tds}</div>
                          <div>Targets: {player.targets}</div>
                          <div>Y/G: {player.yardsPerGame}</div>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div style={{ 
            backgroundColor: '#404040', 
            padding: '1rem', 
            borderRadius: '4px',
            marginTop: '1.5rem'
          }}>
            <h3 style={{ color: '#ffffff', marginTop: 0, marginBottom: '1rem' }}>Injury Report</h3>
            <div style={{
              display: 'grid',
              gridTemplateColumns: '1fr 1fr',
              gap: '1.5rem'
            }}>
              <div>
                <h4 style={{ color: '#ffffff', fontSize: '0.9rem', marginBottom: '1rem' }}>{game.teams.away}</h4>
                <div style={{ 
                  backgroundColor: '#353535', 
                  padding: '1rem', 
                  borderRadius: '4px',
                  borderLeft: '3px solid #ff4a4a'
                }}>
                  {[
                    { player: 'John Smith', position: 'WR', status: 'Questionable', injury: 'Ankle' },
                    { player: 'Mike Johnson', position: 'RB', status: 'Out', injury: 'Knee' },
                    { player: 'Chris Davis', position: 'CB', status: 'Doubtful', injury: 'Hamstring' }
                  ].map((injury, index) => (
                    <div key={index} style={{ marginBottom: '0.75rem' }}>
                      <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                        <span style={{ fontSize: '0.85rem', fontWeight: 'bold', color: '#ffffff' }}>
                          {injury.player} ({injury.position})
                        </span>
                        <span style={{ 
                          fontSize: '0.75rem', 
                          color: injury.status === 'Out' ? '#ff4a4a' : 
                                 injury.status === 'Doubtful' ? '#e6c619' : '#4daf7c'
                        }}>
                          {injury.status}
                        </span>
                      </div>
                      <div style={{ fontSize: '0.8rem', color: '#b0b0b0', marginTop: '0.25rem' }}>
                        {injury.injury}
                      </div>
                    </div>
                  ))}
                </div>
              </div>
              <div>
                <h4 style={{ color: '#ffffff', fontSize: '0.9rem', marginBottom: '1rem' }}>{game.teams.home}</h4>
                <div style={{ 
                  backgroundColor: '#353535', 
                  padding: '1rem', 
                  borderRadius: '4px',
                  borderLeft: '3px solid #ff4a4a'
                }}>
                  {[
                    { player: 'David Wilson', position: 'QB', status: 'Probable', injury: 'Shoulder' },
                    { player: 'James Brown', position: 'TE', status: 'Out', injury: 'Concussion' },
                    { player: 'Robert Taylor', position: 'LB', status: 'Questionable', injury: 'Groin' }
                  ].map((injury, index) => (
                    <div key={index} style={{ marginBottom: '0.75rem' }}>
                      <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                        <span style={{ fontSize: '0.85rem', fontWeight: 'bold', color: '#ffffff' }}>
                          {injury.player} ({injury.position})
                        </span>
                        <span style={{ 
                          fontSize: '0.75rem', 
                          color: injury.status === 'Out' ? '#ff4a4a' : 
                                 injury.status === 'Doubtful' ? '#e6c619' : '#4daf7c'
                        }}>
                          {injury.status}
                        </span>
                      </div>
                      <div style={{ fontSize: '0.8rem', color: '#b0b0b0', marginTop: '0.25rem' }}>
                        {injury.injury}
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};
// NFL Schedule component
const NFLSchedule = () => {
  const [selectedGame, setSelectedGame] = useState(null);
  return (
    <div style={{
      backgroundColor: '#000000',
      padding: '1.5rem',
      borderRadius: '8px',
      border: '2px solid #ff6b00',
      boxShadow: '0 0 10px rgba(255, 107, 0, 0.3)',
    }}>
      <h2 style={{ margin: '0 0 1rem 0', color: '#ffffff' }}>NFL Week 15 Schedule</h2>
      <div style={{ 
        color: '#e0e0e0',
        display: 'flex',
        flexDirection: 'column',
        gap: '0.75rem',
        fontSize: '0.9rem',
      }}>
        <div style={{ borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>
          <div style={{ color: '#9e9e9e', fontSize: '0.8rem', marginBottom: '0.25rem' }}>Thu 12/14</div>
          <a 
            href="#"
            onClick={(e) => {
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Chargers', home: 'Raiders' },
                date: 'Thursday, December 14',
                time: '8:15 PM ET',
                venue: 'Allegiant Stadium, Las Vegas, NV',
                broadcast: 'Prime Video',
                weather: '55°F, Clear',
                records: {
                  away: '5-8',
                  home: '5-8'
                }
              });
            }}
            style={{
              color: '#e0e0e0',
              textDecoration: 'none',
              display: 'block',
              padding: '0.25rem',
              margin: '-0.25rem',
              borderRadius: '4px',
              transition: 'background-color 0.3s ease',
            }}
            onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
            onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
          >
            Chargers @ Raiders - 8:15 PM ET
          </a>
        </div>
        <div style={{ borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>
          <div style={{ color: '#9e9e9e', fontSize: '0.8rem', marginBottom: '0.25rem' }}>Sat 12/16</div>
          <a 
            href="#"
            onClick={(e) => {
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Vikings', home: 'Bengals' },
                date: 'Saturday, December 16',
                time: '1:00 PM ET',
                venue: 'Paycor Stadium, Cincinnati, OH',
                broadcast: 'NFL Network',
                weather: '48°F, Partly Cloudy',
                records: {
                  away: '7-6',
                  home: '7-6'
                }
              });
            }}
            style={{
              color: '#e0e0e0',
              textDecoration: 'none',
              display: 'block',
              padding: '0.25rem',
              margin: '-0.25rem',
              borderRadius: '4px',
              transition: 'background-color 0.3s ease',
            }}
            onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
            onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
          >
            Vikings @ Bengals - 1:00 PM ET
          </a>
          <a 
            href="#"
            onClick={(e) => {
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Steelers', home: 'Colts' },
                date: 'Saturday, December 16',
                time: '4:30 PM ET',
                venue: 'Lucas Oil Stadium, Indianapolis, IN',
                broadcast: 'NFL Network',
                weather: 'Dome',
                records: {
                  away: '7-6',
                  home: '7-6'
                }
              });
            }}
            style={{
              color: '#e0e0e0',
              textDecoration: 'none',
              display: 'block',
              padding: '0.25rem',
              margin: '-0.25rem',
              borderRadius: '4px',
              transition: 'background-color 0.3s ease',
            }}
            onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
            onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
          >
            Steelers @ Colts - 4:30 PM ET
          </a>
          <a 
            href="#"
            onClick={(e) => {
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Broncos', home: 'Lions' },
                date: 'Saturday, December 16',
                time: '8:15 PM ET',
                venue: 'Ford Field, Detroit, MI',
                broadcast: 'NFL Network',
                weather: 'Dome',
                records: {
                  away: '7-6',
                  home: '9-4'
                }
              });
            }}
            style={{
              color: '#e0e0e0',
              textDecoration: 'none',
              display: 'block',
              padding: '0.25rem',
              margin: '-0.25rem',
              borderRadius: '4px',
              transition: 'background-color 0.3s ease',
            }}
            onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
            onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
          >
            Broncos @ Lions - 8:15 PM ET
          </a>
        </div>
        <div style={{ borderBottom: '1px solid #404040', paddingBottom: '0.5rem' }}>
          <div style={{ color: '#9e9e9e', fontSize: '0.8rem', marginBottom: '0.25rem' }}>Sun 12/17</div>
          <a href="#" onClick={(e) => { 
              e.preventDefault(); 
              setSelectedGame({
                teams: { away: 'Falcons', home: 'Panthers' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Bank of America Stadium, Charlotte, NC',
                broadcast: 'FOX',
                weather: '52°F, Sunny',
                records: {
                  away: '6-7',
                  home: '1-12'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Falcons @ Panthers - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Bears', home: 'Browns' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Cleveland Browns Stadium, Cleveland, OH',
                broadcast: 'FOX',
                weather: '44°F, Cloudy',
                records: {
                  away: '5-8',
                  home: '8-5'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Bears @ Browns - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Buccaneers', home: 'Packers' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Lambeau Field, Green Bay, WI',
                broadcast: 'CBS',
                weather: '38°F, Snow Showers',
                records: {
                  away: '6-7',
                  home: '6-7'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Buccaneers @ Packers - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Texans', home: 'Titans' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Nissan Stadium, Nashville, TN',
                broadcast: 'CBS',
                weather: '58°F, Partly Cloudy',
                records: {
                  away: '7-6',
                  home: '5-8'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Texans @ Titans - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Jets', home: 'Dolphins' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Hard Rock Stadium, Miami Gardens, FL',
                broadcast: 'CBS',
                weather: '78°F, Sunny',
                records: {
                  away: '5-8',
                  home: '9-4'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Jets @ Dolphins - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Chiefs', home: 'Patriots' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Gillette Stadium, Foxborough, MA',
                broadcast: 'FOX',
                weather: '42°F, Cloudy',
                records: {
                  away: '8-5',
                  home: '3-10'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Chiefs @ Patriots - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Giants', home: 'Saints' },
                date: 'Sunday, December 17',
                time: '1:00 PM ET',
                venue: 'Caesars Superdome, New Orleans, LA',
                broadcast: 'FOX',
                weather: 'Dome',
                records: {
                  away: '5-8',
                  home: '6-7'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Giants @ Saints - 1:00 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: '49ers', home: 'Cardinals' },
                date: 'Sunday, December 17',
                time: '4:05 PM ET',
                venue: 'State Farm Stadium, Glendale, AZ',
                broadcast: 'CBS',
                weather: 'Dome',
                records: {
                  away: '10-3',
                  home: '3-10'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            49ers @ Cardinals - 4:05 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Commanders', home: 'Rams' },
                date: 'Sunday, December 17',
                time: '4:05 PM ET',
                venue: 'SoFi Stadium, Inglewood, CA',
                broadcast: 'CBS',
                weather: 'Dome',
                records: {
                  away: '4-9',
                  home: '6-7'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Commanders @ Rams - 4:05 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Cowboys', home: 'Bills' },
                date: 'Sunday, December 17',
                time: '4:25 PM ET',
                venue: 'Highmark Stadium, Orchard Park, NY',
                broadcast: 'FOX',
                weather: '40°F, Snow',
                records: {
                  away: '10-3',
                  home: '7-6'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Cowboys @ Bills - 4:25 PM ET
          </a>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Ravens', home: 'Jaguars' },
                date: 'Sunday, December 17',
                time: '8:20 PM ET',
                venue: 'EverBank Stadium, Jacksonville, FL',
                broadcast: 'NBC',
                weather: '65°F, Clear',
                records: {
                  away: '10-3',
                  home: '8-5'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Ravens @ Jaguars - 8:20 PM ET
          </a>
        </div>
        <div>
          <div style={{ color: '#9e9e9e', fontSize: '0.8rem', marginBottom: '0.25rem' }}>Mon 12/18</div>
          <a href="#" onClick={(e) => { 
              e.preventDefault();
              setSelectedGame({
                teams: { away: 'Eagles', home: 'Seahawks' },
                date: 'Monday, December 18',
                time: '8:15 PM ET',
                venue: 'Lumen Field, Seattle, WA',
                broadcast: 'ESPN/ABC',
                weather: '45°F, Light Rain',
                records: {
                  away: '10-3',
                  home: '6-7'
                }
              });
            }}
             style={{ color: '#e0e0e0', textDecoration: 'none', display: 'block', padding: '0.25rem',
                      margin: '-0.25rem', borderRadius: '4px', transition: 'background-color 0.3s ease' }}
             onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
             onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}>
            Eagles @ Seahawks - 8:15 PM ET
          </a>
        </div>
      </div>
      {selectedGame && (
        <GameDetailsModal
          game={selectedGame}
          onClose={() => setSelectedGame(null)}
        />
      )}
    </div>
  );
};
// Teams dropdown component
const TeamDropdown = () => {
  const [isOpen, setIsOpen] = useState(false);
  
  const teams = [
    { id: 1, name: "Buddy's Ballerz", logoPlaceholder: "BBZ" },
    { id: 2, name: "The Canadian Cuckoo Birds", logoPlaceholder: "CCB" },
    { id: 3, name: "The Champions", logoPlaceholder: "CHP" },
    { id: 4, name: "NPCD's Nutz", logoPlaceholder: "NPN" },
    { id: 5, name: "Orange Crushin' It!", logoPlaceholder: "OCI" },
    { id: 6, name: "The Redskins", logoPlaceholder: "RED" },
    { id: 7, name: "The Rock Island Independents", logoPlaceholder: "RII" },
    { id: 8, name: "The Scrap Dogz", logoPlaceholder: "SDZ" }
  ];
  const handleTeamClick = (teamId) => {
    console.log(`Team ${teamId} clicked`);
    // Add team navigation logic here
    setIsOpen(false);
  };
  return (
    <div style={{ position: 'relative' }}>
      <button
        onClick={() => setIsOpen(!isOpen)}
        onBlur={() => setTimeout(() => setIsOpen(false), 200)}
        style={{
          backgroundColor: 'transparent',
          border: 'none',
          color: '#ffffff',
          cursor: 'pointer',
          padding: '0.5rem',
          display: 'flex',
          alignItems: 'center',
          gap: '0.5rem',
        }}
      >
        Teams
        <span style={{
          transform: `rotate(${isOpen ? '180deg' : '0deg'})`,
          transition: 'transform 0.3s ease',
        }}>▾</span>
      </button>
      
      {isOpen && (
        <div style={{
          position: 'absolute',
          top: '100%',
          left: '50%',
          transform: 'translateX(-50%)',
          backgroundColor: '#2d2d2d',
          borderRadius: '4px',
          boxShadow: '0 4px 6px rgba(0, 0, 0, 0.1)',
          width: '200px',
          zIndex: 1000,
          padding: '0.5rem 0',
          marginTop: '0.5rem',
        }}>
          {teams.map((team) => (
            <button
              key={team.id}
              onClick={() => handleTeamClick(team.id)}
              style={{
                width: '100%',
                padding: '0.75rem 1rem',
                backgroundColor: 'transparent',
                border: 'none',
                color: '#ffffff',
                cursor: 'pointer',
                textAlign: 'left',
                transition: 'background-color 0.3s ease',
                display: 'flex',
                alignItems: 'center',
                gap: '0.75rem',
              }}
              onMouseEnter={(e) => e.target.style.backgroundColor = '#404040'}
              onMouseLeave={(e) => e.target.style.backgroundColor = 'transparent'}
            >
              <div style={{
                width: '24px',
                height: '24px',
                backgroundColor: '#404040',
                borderRadius: '50%',
                display: 'flex',
                alignItems: 'center',
                justifyContent: 'center',
                fontSize: '0.7rem',
                fontWeight: 'bold',
              }}>
                {team.logoPlaceholder}
              </div>
              {team.name}
            </button>
          ))}
        </div>
      )}
    </div>
  );
};
const container = document.getElementById('renderDiv');
const root = ReactDOM.createRoot(container);
root.render(<App />);
