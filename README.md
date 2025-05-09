cover={
  showThumbnails && dashboard.thumbnail_url ? (
    <img
      src={dashboard.thumbnail_url}
      alt="Dashboard Thumbnail"
      style={{
        width: '100%',
        height: '150px',
        objectFit: 'cover',
        borderRadius: '4px',
      }}
    />
  ) : null
}
