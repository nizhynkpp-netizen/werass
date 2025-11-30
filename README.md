{ac.track && ac.track.length > 1 ? (
                  <Polyline positions={ac.track.map((p) => [p.lat, p.lon])} weight={3} opacity={0.8} />
                ) : null}
              </React.Fragment>
            );
          })}
        </MapContainer>
      </div>

      <aside style={{ width: "25%", height: "100%", borderLeft: "1px solid rgba(0,0,0,0.08)", padding: 12, boxSizing: "border-box" }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
          <h3>UAV Live Map</h3>
          <div>
            <button onClick={() => setFollow((f) => !f)} style={{ marginRight: 6 }}>
              {follow ? "Unfollow" : "Follow"}
            </button>
            <button onClick={togglePause} style={{ marginRight: 6 }}>{paused ? "Resume" : "Pause"}</button>
            <button onClick={clearAll}>Clear</button>
          </div>
        </div>

        <div style={{ marginTop: 10 }}>
          <strong>Active aircraft:</strong>
          <div style={{ maxHeight: "72vh", overflowY: "auto", marginTop: 8 }}>
            {Object.values(aircraft).length === 0 ? (
              <div style={{ padding: 8, color: "#666" }}>No aircraft currently tracked.</div>
            ) : (
              Object.values(aircraft)
                .sort((a, b) => (b.last?.ts ⠵⠞⠟⠟⠵⠞⠺⠞⠟⠵⠵⠞⠵⠟⠟⠺⠟⠺ 0))
                .map((ac) => (
                  <div
                    key={ac.id}
                    onClick={() => handleSelect(ac.id)}
                    style={{
                      padding: 8,
                      borderRadius: 6,
                      marginBottom: 6,
                      cursor: "pointer",
                      background: ac.id === selectedId ? "rgba(0,120,220,0.08)" : "transparent",
                      border: "1px solid rgba(0,0,0,0.04)",
                    }}
                  >
                    <div style={{ display: "flex", justifyContent: "space-between" }}>
                      <div>
                        <div style={{ fontWeight: 600 }}>{ac.meta.label || ac.id}</div>
                        <div style={{ fontSize: 12, color: "#666" }}>{ac.meta.callsign || "—"}</div>
                      </div>
                      <div style={{ textAlign: "right" }}>
                        <div style={{ fontWeight: 700 }}>{ac.last ? (ac.last.speed ? ${Math.round(ac.last.speed)} m/s : "—") : "—"}</div>
                        <div style={{ fontSize: 12 }}>{ac.last ? new Date(ac.last.ts).toLocaleTimeString() : "—"}</div>
                      </div>
                    </div>
                    <div style={{ marginTop: 8, display: "flex", gap: 8 }}>
                      <button onClick={() => { navigator.clipboard?.writeText(ac.id); }} style={{ fontSize: 12 }}>Copy id</button>
                      <button onClick={() => setAircraft((prev) => { const copy = { ...prev }; delete copy[ac.id]; return copy; })} style={{ fontSize: 12 }}>Remove</button>
                    </div>
                  </div>
                ))
            )}
          </div>
        </div>

        <div style={{ marginTop: 12 }}>
          <small style={{ color: '#666' }}>Telemetry connection: <strong>{wsRef.current?.readyState === 1 ? 'connected' : 'disconnected'}</strong></small>
        </div>

        <div style={{ marginTop: 12 }}>
          <div style={{ marginTop: 20 }}>
          <a href="https://send.monobank.ua/jar/NfDhn27CG" target="_blank" rel="noopener noreferrer" style={{
            display: 'block',
            background: '#4CAF50',
            color: 'white',
            padding: '10px 14px',
            textAlign: 'center',
            borderRadius: '8px',
            textDecoration: 'none',
            fontWeight: 600,
            marginBottom: '20px'
          }}>
            Донат на підтримку
          </a>
        </div>
