# hello-world
This is a simple repository.

CDate			CDate::now( bool bLocalTime /*= false*/ )
{
#if 1
	// Get time using boost, which can give us millisecond accuracy (or more!).
	const boost::posix_time::ptime t = ( bLocalTime ? boost::posix_time::microsec_clock::local_time() : boost::posix_time::microsec_clock::universal_time() );
	const boost::gregorian::date d = t.date();
	const boost::posix_time::time_duration tod = t.time_of_day();
	return CDate
		( 
			d.year(),
			d.month(),
			d.day(),
			tod.hours(),
			tod.minutes(),
			tod.seconds(),
			( tod.total_milliseconds() % 1000 ) /* slightly tiresome */
		);
#else
	time_t currentTime;
	::time( &currentTime );					// Current system time.
	tm * pUTC = gmtime( &currentTime );		// Convert to UTC.
	return CDate( 1900 + pUTC->tm_year, 1 + pUTC->tm_mon, pUTC->tm_mday, pUTC->tm_hour, pUTC->tm_min, pUTC->tm_sec );
#endif
}
