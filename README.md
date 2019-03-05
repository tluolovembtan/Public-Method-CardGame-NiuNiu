
/**

	* 牛牛
	 * 纸牌游戏牛牛的最优算法
	 * 1.计算5张牌(10-K都是0)之和，获得余数X
	 * 2.查询组成余数X的2张牌(策划穷举，X为1-9全部只有45种牌)是否存在，高大上说话用正则表达式
	 * 3.存在即有牛，余数为牛X
	 * 五小牛 > 五花牛 > 牛牛
	 */
   
	 public function niu()
	 {
		header("Content-Type: text/html;charset=utf-8");
		
		$res = M('niu')->select();
		//10所对应的id
		$tun_id = array(37,38,39,40);
		
		//取第一人牌
		$seek = $this->randarr($res, 5);//随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_one = array();
		$chess_one = array();
		$more_one = array();
		foreach ($seek as $k => $v) {
			 $seek_one[] = $v['id'];
			 $chess_one[] = $v['niu'];
			 $more_one[] = $v['size'];
		}
		foreach ($res as $key => $val) {
			if (in_array($val['id'], $seek_one)) {
				unset($res[$key]);
			}
		}
		$res_one = array_values($res);//重置键名剩余47张牌
		
		$sum_one = 0;
		$sum_one_1 = 0;
		for($i = 0; $i < count($seek); $i++) {
			$sum_one += $seek[$i]['sorce'];
			if ($seek[$i]['sorce'] == 0) {
				$seek[$i]['sorce'] = 10;
			}
			$sum_one_1 += $seek[$i]['sorce'];
		}
		
		if ($sum_one <= 10 && $sum_one_1 <= 10) {
			$n_one = "五小牛"."\n";
		} else if ($sum_one_1 == 50 && !in_array($seek_one[0], $tun_id) && !in_array($seek_one[1], $tun_id) && !in_array($seek_one[2], $tun_id) && !in_array($seek_one[3], $tun_id) && !in_array($seek_one[4], $tun_id)) {//判断里面是否有10a,10b,10c,10d
			$n_one = "五花牛"."\n";
		} else {
			$remain_one = intval($sum_one%10);//求余
			if ($remain_one == 0) {
				$n_one = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_one = array();
				foreach ($seek as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_one[] = $v['sorce'];
				}
				
				$m_one = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_one[$i] + $nums_one[$j])%10) == $remain_one) {
							//正常情况
							$m_one++;
						}
					}
				}
				if ($m_one == 0) {
					$n_one = "无牛"."\n";
					$n_than_one = "";
					//无牛取最大的那张牌
					$n_more_one = $this->getMax($more_one);
				} else {
					$n_one = "有牛，牛".$remain_one."\n";
					$n_than_one = $remain_one;
				}
			}
		}
		
	//------------------------------------------------------------------------------------------------------------------------------------------------------	
		
		
		//取第二人牌
		$seek_2 = $this->randarr($res_one, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_two = array();
		$chess_two = array();
		$more_two = array();
		foreach ($seek_2 as $k => $v) {
			 $seek_two[] = $v['id'];
			 $chess_two[] = $v['niu'];
			 $more_two[] = $v['size'];
		}
		foreach ($res_one as $key => $val) {
			if (in_array($val['id'], $seek_two)) {
				unset($res_one[$key]);
			}
		}
		$res_two = array_values($res_one);//重置键名剩余42张牌
		
		$sum_two = 0;
		$sum_two_2 = 0;
		for($i = 0; $i < count($seek_2); $i++) {
			$sum_two += $seek_2[$i]['sorce'];
			if ($seek_2[$i]['sorce'] == 0) {
				$seek_2[$i]['sorce'] = 10;
			}
			$sum_two_2 += $seek_2[$i]['sorce'];
		}
		if ($sum_two <= 10 && $sum_two_2 <= 10) {
			$n_two = "五小牛"."\n";
		} else if ($sum_two_2 == 50 && !in_array($seek_two[0], $tun_id) && !in_array($seek_two[1], $tun_id) && !in_array($seek_two[2], $tun_id) && !in_array($seek_two[3], $tun_id) && !in_array($seek_two[4], $tun_id)) {
			$n_two = "五花牛"."\n";
		} else {
			$remain_two = intval($sum_two%10);//求余
			if ($remain_two == 0) {
				$n_two = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_two = array();
				foreach ($seek_2 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_two[] = $v['sorce'];
				}
				
				$m_two = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_two[$i] + $nums_two[$j])%10) == $remain_two) {
							//正常情况
							$m_two++;
						}
					}
				}
				if ($m_two == 0) {
					$n_two = "无牛"."\n";
					$n_than_two = "";
					//无牛取最大的那张牌
					$n_more_two = $this->getMax($more_two);
				} else {
					$n_two = "有牛，牛".$remain_two."\n";
					$n_than_two = $remain_two;
				}
			}
		}
		
	//---------------------------------------------------------------------------------------------------------------------------------------------	
		
	
		//取第三人牌
		$seek_3 = $this->randarr($res_two, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_three = array();
		$chess_three = array();
		$more_three = array();
		foreach ($seek_3 as $k => $v) {
			 $seek_three[] = $v['id'];
			 $chess_three[] = $v['niu'];
			 $more_three[] = $v['size'];
		}
		foreach ($res_two as $key => $val) {
			if (in_array($val['id'], $seek_three)) {
				unset($res_two[$key]);
			}
		}
		$res_three = array_values($res_two);//重置键名剩余37张牌
		
		$sum_three = 0;
		$sum_three_3 = 0;
		for($i = 0; $i < count($seek_3); $i++) {
			$sum_three += $seek_3[$i]['sorce'];
			if ($seek_3[$i]['sorce'] == 0) {
				$seek_3[$i]['sorce'] = 10;
			}
			$sum_three_3 += $seek_3[$i]['sorce'];
		}
		if ($sum_three <= 10 && $sum_three_3 <= 10) {
			$n_three = "五小牛"."\n";
		} else if ($sum_three_3 == 50 && !in_array($seek_three[0], $tun_id) && !in_array($seek_three[1], $tun_id) && !in_array($seek_three[2], $tun_id) && !in_array($seek_three[3], $tun_id) && !in_array($seek_three[4], $tun_id)) {
			$n_three = "五花牛"."\n";
		} else {
			$remain_three = intval($sum_three%10);//求余
			if ($remain_three == 0) {
				$n_three = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_three = array();
				foreach ($seek_3 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_three[] = $v['sorce'];
				}
				
				$m_three = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_three[$i] + $nums_three[$j])%10) == $remain_three) {
							//正常情况
							$m_three++;
						}
					}
				}
				if ($m_three == 0) {
					$n_three = "无牛"."\n";
					$n_than_three = "";
					//无牛取最大的那张牌
					$n_more_three = $this->getMax($more_three);
				} else {
					$n_three = "有牛，牛".$remain_three."\n";
					$n_than_three = $remain_three;
				}
			}
		}
		
	//----------------------------------------------------------------------------------------------------------------------------------------------	
		
	
		//取第四人牌
		$seek_4 = $this->randarr($res_three, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_four = array();
		$chess_four = array();
		$more_four = array();
		foreach ($seek_4 as $k => $v) {
			 $seek_four[] = $v['id'];
			 $chess_four[] = $v['niu'];
			 $more_four[] = $v['size'];
		}
		foreach ($res_three as $key => $val) {
			if (in_array($val['id'], $seek_four)) {
				unset($res_three[$key]);
			}
		}
		$res_four = array_values($res_three);//重置键名剩余32张牌
		
		$sum_four = 0;
		$sum_four_4 = 0;
		for($i = 0; $i < count($seek_4); $i++) {
			$sum_four += $seek_4[$i]['sorce'];
			if ($seek_4[$i]['sorce'] == 0) {
				$seek_4[$i]['sorce'] = 10;
			}
			$sum_four_4 += $seek_4[$i]['sorce'];
		}
		if ($sum_four <= 10 && $sum_four_4 <= 10) {
			$n_four = "五小牛"."\n";
		} else if ($sum_four_4 == 50 && !in_array($seek_four[0], $tun_id) && !in_array($seek_four[1], $tun_id) && !in_array($seek_four[2], $tun_id) && !in_array($seek_four[3], $tun_id) && !in_array($seek_four[4], $tun_id)) {
			$n_four = "五花牛"."\n";
		} else {
			$remain_four = intval($sum_four%10);//求余
			if ($remain_four == 0) {
				$n_four = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_four = array();
				foreach ($seek_4 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_four[] = $v['sorce'];
				}
				
				$m_four = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_four[$i] + $nums_four[$j])%10) == $remain_four) {
							//正常情况
							$m_four++;
						}
					}
				}
				if ($m_four == 0) {
					$n_four = "无牛"."\n";
					$n_than_four = "";
					//无牛取最大的那张牌
					$n_more_four = $this->getMax($more_four);
				} else {
					$n_four = "有牛，牛".$remain_four."\n";
					$n_than_four = $remain_four;
				}
			}
		}
		
	//-------------------------------------------------------------------------------------------------------------------------------------------	
		
		//取第五人牌
		$seek_5 = $this->randarr($res_four, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_five = array();
		$chess_five = array();
		$more_five = array();
		foreach ($seek_5 as $k => $v) {
			 $seek_five[] = $v['id'];
			 $chess_five[] = $v['niu'];
			 $more_five[] = $v['size'];
		}
		foreach ($res_four as $key => $val) {
			if (in_array($val['id'], $seek_five)) {
				unset($res_four[$key]);
			}
		}
		$res_five = array_values($res_four);//重置键名剩余27张牌
		
		$sum_five = 0;
		$sum_five_5 = 0;
		for($i = 0; $i < count($seek_5); $i++) {
			$sum_five += $seek_5[$i]['sorce'];
			if ($seek_5[$i]['sorce'] == 0) {
				$seek_5[$i]['sorce'] = 10;
			}
			$sum_five_5 += $seek_5[$i]['sorce'];
		}
		if ($sum_five <= 10 && $sum_five_5 <= 10) {
			$n_five = "五小牛"."\n";
		} else if ($sum_five_5 == 50 && !in_array($seek_five[0], $tun_id) && !in_array($seek_five[1], $tun_id) && !in_array($seek_five[2], $tun_id) && !in_array($seek_five[3], $tun_id) && !in_array($seek_five[4], $tun_id)) {
			$n_five = "五花牛"."\n";
		} else {
			$remain_five = intval($sum_five%10);//求余
			if ($remain_five == 0) {
				$n_five = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_five = array();
				foreach ($seek_5 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_five[] = $v['sorce'];
				}
				
				$m_five = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_five[$i] + $nums_five[$j])%10) == $remain_five) {
							//正常情况
							$m_five++;
						}
					}
				}
				if ($m_five == 0) {
					$n_five = "无牛"."\n";
					$n_than_five = "";
					//无牛取最大的那张牌
					$n_more_five = $this->getMax($more_five);
				} else {
					$n_five = "有牛，牛".$remain_five."\n";
					$n_than_five = $remain_five;
				}
			}
		}
		
	//---------------------------------------------------------------------------------------------------------------------------------------------------	
		
		//取第六人牌
		$seek_6 = $this->randarr($res_five, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_six = array();
		$chess_six = array();
		$more_six = array();
		foreach ($seek_6 as $k => $v) {
			 $seek_six[] = $v['id'];
			 $chess_six[] = $v['niu'];
			 $more_six[] = $v['size'];
		}
		foreach ($res_five as $key => $val) {
			if (in_array($val['id'], $seek_six)) {
				unset($res_five[$key]);
			}
		}
		$res_six = array_values($res_five);//重置键名剩余22张牌
		
		$sum_six = 0;
		$sum_six_6 = 0;
		for($i = 0; $i < count($seek_6); $i++) {
			$sum_six += $seek_6[$i]['sorce'];
			if ($seek_6[$i]['sorce'] == 0) {
				$seek_6[$i]['sorce'] = 10;
			}
			$sum_six_6 += $seek_6[$i]['sorce'];
		}
		if ($sum_six <= 10 && $sum_six_6 <= 10) {
			$n_six = "五小牛"."\n";
		} else if ($sum_six_6 == 50 && !in_array($seek_six[0], $tun_id) && !in_array($seek_six[1], $tun_id) && !in_array($seek_six[2], $tun_id) && !in_array($seek_six[3], $tun_id) && !in_array($seek_six[4], $tun_id)) {
			$n_six = "五花牛"."\n";
		} else {
			$remain_six = intval($sum_six%10);//求余
			if ($remain_six == 0) {
				$n_six = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_six = array();
				foreach ($seek_6 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_six[] = $v['sorce'];
				}
				
				$m_six = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_six[$i] + $nums_six[$j])%10) == $remain_six) {
							//正常情况
							$m_six++;
						}
					}
				}
				if ($m_six == 0) {
					$n_six = "无牛"."\n";
					$n_than_six = "";
					//无牛取最大的那张牌
					$n_more_six = $this->getMax($more_six);
				} else {
					$n_six = "有牛，牛".$remain_six."\n";
					$n_than_six = $remain_six;
				}
			}
		}
		
	//--------------------------------------------------------------------------------------------------------------------------------------------	
		
		//取第七人牌
		$seek_7 = $this->randarr($res_six, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_seven = array();
		$chess_seven = array();
		$more_seven = array();
		foreach ($seek_7 as $k => $v) {
			 $seek_seven[] = $v['id'];
			 $chess_seven[] = $v['niu'];
			 $more_seven[] = $v['size'];
		}
		foreach ($res_six as $key => $val) {
			if (in_array($val['id'], $seek_seven)) {
				unset($res_six[$key]);
			}
		}
		$res_seven = array_values($res_six);//重置键名剩余17张牌
		
		$sum_seven = 0;
		$sum_seven_7 = 0;
		for($i = 0; $i < count($seek_7); $i++) {
			$sum_seven += $seek_7[$i]['sorce'];
			if ($seek_7[$i]['sorce'] == 0) {
				$seek_7[$i]['sorce'] = 10;
			}
			$sum_seven_7 += $seek_7[$i]['sorce'];
		}
		if ($sum_seven <= 10 && $sum_seven_7 <= 10) {
			$n_seven = "五小牛"."\n";
		} else if ($sum_seven_7 == 50 && !in_array($seek_seven[0], $tun_id) && !in_array($seek_seven[1], $tun_id) && !in_array($seek_seven[2], $tun_id) && !in_array($seek_seven[3], $tun_id) && !in_array($seek_seven[4], $tun_id)) {
			$n_seven = "五花牛"."\n";
		} else {
			$remain_seven = intval($sum_seven%10);//求余
			if ($remain_seven == 0) {
				$n_seven = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_seven = array();
				foreach ($seek_7 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_seven[] = $v['sorce'];
				}
				
				$m_seven = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_seven[$i] + $nums_seven[$j])%10) == $remain_seven) {
							//正常情况
							$m_seven++;
						}
					}
				}
				if ($m_seven == 0) {
					$n_seven = "无牛"."\n";
					$n_than_seven = "";
					//无牛取最大的那张牌
					$n_more_seven = $this->getMax($more_seven);
				} else {
					$n_seven = "有牛，牛".$remain_seven."\n";
					$n_than_seven = $remain_seven;
				}
			}
		}
		
	//-------------------------------------------------------------------------------------------------------------------------------------	
		
		//取第八人牌
		$seek_8 = $this->randarr($res_seven, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_eight = array();
		$chess_eight = array();
		$more_eight = array();
		foreach ($seek_8 as $k => $v) {
			 $seek_eight[] = $v['id'];
			 $chess_eight[] = $v['niu'];
			 $more_eight[] = $v['size'];
		}
		foreach ($res_seven as $key => $val) {
			if (in_array($val['id'], $seek_eight)) {
				unset($res_seven[$key]);
			}
		}
		$res_eight = array_values($res_seven);//重置键名剩余12张牌
		
		$sum_eight = 0;
		$sum_eight_8 = 0;
		for($i = 0; $i < count($seek_8); $i++) {
			$sum_eight += $seek_8[$i]['sorce'];
			if ($seek_8[$i]['sorce'] == 0) {
				$seek_8[$i]['sorce'] = 10;
			}
			$sum_eight_8 += $seek_8[$i]['sorce'];
		}
		if ($sum_eight <= 10 && $sum_eight_8 <= 10) {
			$n_eight = "五小牛"."\n";
		} else if ($sum_eight_8 == 50 && !in_array($seek_eight[0], $tun_id) && !in_array($seek_eight[1], $tun_id) && !in_array($seek_eight[2], $tun_id) && !in_array($seek_eight[3], $tun_id) && !in_array($seek_eight[4], $tun_id)) {
			$n_eight = "五花牛"."\n";
		} else {
			$remain_eight = intval($sum_eight%10);//求余
			if ($remain_eight == 0) {
				$n_eight = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_eight = array();
				foreach ($seek_8 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_eight[] = $v['sorce'];
				}
				
				$m_eight = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_eight[$i] + $nums_eight[$j])%10) == $remain_eight) {
							//正常情况
							$m_eight++;
						}
					}
				}
				if ($m_eight == 0) {
					$n_eight = "无牛"."\n";
					$n_than_eight = "";
					//无牛取最大的那张牌
					$n_more_eight = $this->getMax($more_eight);
				} else {
					$n_eight = "有牛，牛".$remain_eight."\n";
					$n_than_eight = $remain_eight;
				}
			}
		}
		
	//----------------------------------------------------------------------------------------------------------------------------------	
		
		//取第九人牌
		$seek_9 = $this->randarr($res_eight, 5);//再随机取5张牌
		//将随机取的5张牌转一维数组
		$seek_nine = array();
		$chess_nine = array();
		$more_nine = array();
		foreach ($seek_9 as $k => $v) {
			 $seek_nine[] = $v['id'];
			 $chess_nine[] = $v['niu'];
			 $more_nine[] = $v['size'];
		}
		foreach ($res_eight as $key => $val) {
			if (in_array($val['id'], $seek_nine)) {
				unset($res_eight[$key]);
			}
		}
		$res_nine = array_values($res_eight);//重置键名剩余7张牌
		
		$sum_nine = 0;
		$sum_nine_9 = 0;
		for($i = 0; $i < count($seek_9); $i++) {
			$sum_nine += $seek_9[$i]['sorce'];
			if ($seek_9[$i]['sorce'] == 0) {
				$seek_9[$i]['sorce'] = 10;
			}
			$sum_nine_9 += $seek_9[$i]['sorce'];
		}
		if ($sum_nine <= 10 && $sum_nine_9 <= 10) {
			$n_nine = "五小牛"."\n";
		} else if ($sum_nine_9 == 50 && !in_array($seek_nine[0], $tun_id) && !in_array($seek_nine[1], $tun_id) && !in_array($seek_nine[2], $tun_id) && !in_array($seek_nine[3], $tun_id) && !in_array($seek_nine[4], $tun_id)) {
			$n_nine = "五花牛"."\n";
		} else {
			$remain_nine = intval($sum_nine%10);//求余
			if ($remain_nine == 0) {
				$n_nine = "牛牛"."\n";
			} else {
				//二维数组转一维
				$nums_nine = array();
				foreach ($seek_9 as $k => $v) {
					if ($v['sorce'] == 10) {
						$v['sorce'] = 0;
					}
					$nums_nine[] = $v['sorce'];
				}
				
				$m_nine = 0;
				for($i =0 ; $i < 4 ; $i++){
					for($j = $i + 1; $j < 5; $j++){
						//判断是否存在
						if(intval(($nums_nine[$i] + $nums_nine[$j])%10) == $remain_nine) {
							//正常情况
							$m_nine++;
						}
					}
				}
				if ($m_nine == 0) {
					$n_nine = "无牛"."\n";
					$n_than_nine = "";
					//无牛取最大的那张牌
					$n_more_nine = $this->getMax($more_nine);
				} else {
					$n_nine = "有牛，牛".$remain_nine."\n";
					$n_than_nine = $remain_nine;
				}
			}
		}
		
		
		
		echo "玩家一"."\n";
		print_r($nums_one)."\n";
		print_r($chess_one)."\n";
		echo $sum_one."\n";
		echo $sum_one_1."\n";
		echo $n_one."\n\n";
		
		echo "玩家二"."\n";
		print_r($nums_two)."\n";
		print_r($chess_two)."\n";
		echo $sum_two."\n";
		echo $sum_two_2."\n";
		echo $n_two."\n\n";
		
		echo "玩家三"."\n";
		print_r($nums_three)."\n";
		print_r($chess_three)."\n";
		echo $sum_three."\n";
		echo $sum_three_3."\n";
		echo $n_three."\n\n";
		
		echo "玩家四"."\n";
		print_r($nums_four)."\n";
		print_r($chess_four)."\n";
		echo $sum_four."\n";
		echo $sum_four_4."\n";
		echo $n_four."\n\n";
		
		echo "玩家五"."\n";
		print_r($nums_five)."\n";
		print_r($chess_five)."\n";
		echo $sum_five."\n";
		echo $sum_five_5."\n";
		echo $n_five."\n\n";
		
		echo "玩家六"."\n";
		print_r($nums_six)."\n";
		print_r($chess_six)."\n";
		echo $sum_six."\n";
		echo $sum_six_6."\n";
		echo $n_six."\n\n";
		
		echo "玩家七"."\n";
		print_r($nums_seven)."\n";
		print_r($chess_seven)."\n";
		echo $sum_seven."\n";
		echo $sum_seven_7."\n";
		echo $n_seven."\n\n";
		
		echo "玩家八"."\n";
		print_r($nums_eight)."\n";
		print_r($chess_eight)."\n";
		echo $sum_eight."\n";
		echo $sum_eight_8."\n";
		echo $n_eight."\n\n";
		
		echo "玩家九"."\n";
		print_r($nums_nine)."\n";//手牌对应分
		print_r($chess_nine)."\n";//手牌
		echo $sum_nine."\n";//分数
		echo $sum_nine_9."\n";//最大分数
		echo $n_nine."\n\n";//输出牛
		
		print_r($res_nine)."\n";
		
		//比大小
		//五小牛-12 > 五花牛-11 > 牛牛-10 > 牛九-9 > 牛八-8 > 牛七-7 > 牛六-6 > 牛五-5 > 牛四-4 > 牛三-3 > 牛二-2 > 牛一-1 > 无牛-0
		//相同的比最大的那张牌，谁的大谁赢
		//第一步,比牛
		//将玩家、牛、最大那张牌合并数组
		$arr = array(
			array(
				'name' => '玩家一',
				'niu' => $n_than_one ? $n_than_one + 100 : $n_more_one,
				'more' => $n_more_one,
			),
			array(
				'name' => '玩家二',
				'niu' => $n_than_two ? $n_than_two + 100 : $n_more_two,
				'more' => $n_more_two,
			),
			array(
				'name' => '玩家三',
				'niu' => $n_than_three ? $n_than_three + 100 : $n_more_three,
				'more' => $n_more_three,
			),
			array(
				'name' => '玩家四',
				'niu' => $n_than_four ? $n_than_four + 100 : $n_more_four,
				'more' => $n_more_four,
			),
			array(
				'name' => '玩家五',
				'niu' => $n_than_five ? $n_than_five + 100 : $n_more_five,
				'more' => $n_more_five,
			),
			array(
				'name' => '玩家六',
				'niu' => $n_than_six ? $n_than_six + 100 : $n_more_six,
				'more' => $n_more_six,
			),
			array(
				'name' => '玩家七',
				'niu' => $n_than_seven ? $n_than_seven + 100 : $n_more_seven,
				'more' => $n_more_seven,
			),
			array(
				'name' => '玩家八',
				'niu' => $n_than_eight ? $n_than_eight + 100 : $n_more_eight,
				'more' => $n_more_eight,
			),
			array(
				'name' => '玩家九',
				'niu' => $n_than_nine ? $n_than_nine + 100 : $n_more_nine,
				'more' => $n_more_nine,
			)
		);
		print_r($arr);
	 }
	 
	//随机取5张牌
	public function randarr($arr, $num) {
		$arrid = array_rand($arr, $num);
		foreach ($arrid as $v) {
			$newarr [] = $arr [$v];
		}
		return $newarr;
	}
	//php取出数组中的最大值
    public function getMax($arr) {
        $max = $arr[0];
        foreach ($arr as $k => $v) {
            if($v > $max){
                $max = $v;
            }
        }
        return $max;
    }
    
    
    数据表
    
    SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for `niu`
-- ----------------------------
DROP TABLE IF EXISTS `niu`;
CREATE TABLE `niu` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键id',
  `sorce` int(2) NOT NULL COMMENT '对应分数',
  `niu` varchar(3) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=53 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of niu
-- ----------------------------
INSERT INTO `niu` VALUES ('1', '1', '1a');
INSERT INTO `niu` VALUES ('2', '1', '1b');
INSERT INTO `niu` VALUES ('3', '1', '1c');
INSERT INTO `niu` VALUES ('4', '1', '1d');
INSERT INTO `niu` VALUES ('5', '2', '2a');
INSERT INTO `niu` VALUES ('6', '2', '2b');
INSERT INTO `niu` VALUES ('7', '2', '2c');
INSERT INTO `niu` VALUES ('8', '2', '2d');
INSERT INTO `niu` VALUES ('9', '3', '3a');
INSERT INTO `niu` VALUES ('10', '3', '3b');
INSERT INTO `niu` VALUES ('11', '3', '3c');
INSERT INTO `niu` VALUES ('12', '3', '3d');
INSERT INTO `niu` VALUES ('13', '4', '4a');
INSERT INTO `niu` VALUES ('14', '4', '4b');
INSERT INTO `niu` VALUES ('15', '4', '4c');
INSERT INTO `niu` VALUES ('16', '4', '4d');
INSERT INTO `niu` VALUES ('17', '5', '5a');
INSERT INTO `niu` VALUES ('18', '5', '5b');
INSERT INTO `niu` VALUES ('19', '5', '5c');
INSERT INTO `niu` VALUES ('20', '5', '5d');
INSERT INTO `niu` VALUES ('21', '6', '6a');
INSERT INTO `niu` VALUES ('22', '6', '6b');
INSERT INTO `niu` VALUES ('23', '6', '6c');
INSERT INTO `niu` VALUES ('24', '6', '6d');
INSERT INTO `niu` VALUES ('25', '7', '7a');
INSERT INTO `niu` VALUES ('26', '7', '7b');
INSERT INTO `niu` VALUES ('27', '7', '7c');
INSERT INTO `niu` VALUES ('28', '7', '7d');
INSERT INTO `niu` VALUES ('29', '8', '8a');
INSERT INTO `niu` VALUES ('30', '8', '8b');
INSERT INTO `niu` VALUES ('31', '8', '8c');
INSERT INTO `niu` VALUES ('32', '8', '8d');
INSERT INTO `niu` VALUES ('33', '9', '9a');
INSERT INTO `niu` VALUES ('34', '9', '9b');
INSERT INTO `niu` VALUES ('35', '9', '9c');
INSERT INTO `niu` VALUES ('36', '9', '9d');
INSERT INTO `niu` VALUES ('37', '0', '10a');
INSERT INTO `niu` VALUES ('38', '0', '10b');
INSERT INTO `niu` VALUES ('39', '0', '10c');
INSERT INTO `niu` VALUES ('40', '0', '10d');
INSERT INTO `niu` VALUES ('41', '0', 'Ja');
INSERT INTO `niu` VALUES ('42', '0', 'Jb');
INSERT INTO `niu` VALUES ('43', '0', 'Jc');
INSERT INTO `niu` VALUES ('44', '0', 'Jd');
INSERT INTO `niu` VALUES ('45', '0', 'Qa');
INSERT INTO `niu` VALUES ('46', '0', 'Qb');
INSERT INTO `niu` VALUES ('47', '0', 'Qc');
INSERT INTO `niu` VALUES ('48', '0', 'Qd');
INSERT INTO `niu` VALUES ('49', '0', 'Ka');
INSERT INTO `niu` VALUES ('50', '0', 'Kb');
INSERT INTO `niu` VALUES ('51', '0', 'Kc');
INSERT INTO `niu` VALUES ('52', '0', 'Kd');
