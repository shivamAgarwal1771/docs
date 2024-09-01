import React, { useEffect, useState } from 'react';
import { useSelector } from 'react-redux';

const CallTimer = () => {
  const [timers, setTimers] = useState({});

  const callInitiated = useSelector((state) => state.callInitiated);
  const callEnded = useSelector((state) => state.callEnded);

  useEffect(() => {
    if (callInitiated && callInitiated.id) {
      const { id } = callInitiated;
      if (!timers[id]) {
        // Start a new timer for the call
        const intervalId = setInterval(() => {
          setTimers((prevTimers) => ({
            ...prevTimers,
            [id]: (prevTimers[id] || 0) + 1,
          }));
        }, 1000);
        setTimers((prevTimers) => ({
          ...prevTimers,
          [`${id}_interval`]: intervalId,
        }));
      }
    }
  }, [callInitiated, timers]);

  useEffect(() => {
    if (callEnded && callEnded.id) {
      const { id } = callEnded;
      if (timers[`${id}_interval`]) {
        // Stop the timer for the call
        clearInterval(timers[`${id}_interval`]);
        setTimers((prevTimers) => {
          const updatedTimers = { ...prevTimers };
          delete updatedTimers[`${id}_interval`];
          return updatedTimers;
        });
      }
    }
  }, [callEnded, timers]);

  return (
    <div>
      <h1>Call Timers</h1>
      {Object.keys(timers).map((key) => {
        if (key.endsWith('_interval')) return null; // Skip interval IDs
        return (
          <div key={key}>
            Call ID: {key} - Timer: {timers[key]} seconds
          </div>
        );
      })}
    </div>
  );
};

export default CallTimer;
