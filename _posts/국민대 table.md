```d2

전체 테이블 목록{
    shape : sql_table

    apply_train
    resume_certificate
    resume
    resume_education
    resume_language
    company
    recruitment
    sample_submission
}
```

``` d2
    resume <- resume_certificate
    resume <- resume_education
    resume <- resume_language

    resume_certificate {
        shape : sql_table
      resume_seq : object
      certificate_contents : object
    }
    resume {
        shape : sql_table
      resume_seq : object
      reg_date : object
      updated_date : object
      degree : int64
      graduate_date : int64
      hope_salary : float64
      last_salary : float64
      text_keyword : object
      job_code_seq1 : object
      job_code_seq2 : object
      job_code_seq3 : object
      career_month : int64
      career_job_code : object
    }
    resume_education {
        shape : sql_table
      resume_seq : object
      hischool_type_seq : int64
      hischool_special_type : object
      hischool_nation : object
      hischool_gender : object
      hischool_location_seq : int64
      univ_type_seq1 : int64
      univ_type_seq2 : int64
      univ_transfer : int64
      univ_location : int64
      univ_major : object
      univ_sub_major : object
      univ_major_type : int64
      univ_score : float64
    }
    resume_language {
        shape : sql_table
      resume_seq : object
      language : int64
      exam_name : int64
      score : float64
    }


```

```d2

company -> recruitment
company {
    shape : sql_table
  recruitment_seq : object
  company_type_seq : int64
  supply_kind : int64
  employee : int64
}
recruitment {
    shape : sql_table
  recruitment_seq : object
  address_seq1 : float64
  address_seq2 : float64
  address_seq3 : float64
  career_end : int64
  career_start : int64
  check_box_keyword : object
  education : int64
  major_task : int64
  qualifications : int64
  text_keyword : object
}
```
```d2

resume -> apply_train
resume -> sample_submission

recruitment -> apply_train
recruitment -> sample_submission


apply_train {
        shape : sql_table
      resume_seq : object
      recruitment_seq : object
    }
sample_submission {
    shape : sql_table
  resume_seq : object
  recruitment_seq : object
}
```