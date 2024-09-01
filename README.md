import React, { useEffect, useState } from 'react';
import { useSelector } from 'react-redux';

const CallTimer = ({ callId }) => {
  const [timer, setTimer] = useState(0);
  const [intervalId, setIntervalId] = useState(null);

  const call = useSelector((state) =>
    state.calls.find((call) => call.id === callId)
  );

  useEffect(() => {
    if (call && call.status === 'initiated') {
      const id = setInterval(() => {
        setTimer((prevTime) => prevTime + 1);
      }, 1000);
      setIntervalId(id);
    }

    if (call && call.status === 'ended') {
      clearInterval(intervalId);
      setIntervalId(null);
    }

    return () => {
      if (intervalId) clearInterval(intervalId);
    };
  }, [call]);

  return (
    <div>
      Call ID: {callId} - Timer: {timer} seconds
    </div>
  );
};

export default CallTimer;
